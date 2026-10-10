# ARCHITECTURE: Marshal

One Next.js 16 process on Railway (one replica), one Foundry contract on Monad testnet, no database. Built on branch `marshal` of the `itssaharsh/dibs` fork; Dibs stays untouched on `main` and live on its own Railway service.

```
phone (/)  --POST /api/chat-->  Next server ──fetch──> model (OpenAI-compatible, free tier) ─┐
                                   │  ◄── reply + pay(to, amount, reason) tool call ──────────┘
                                   │  context JSON → keccak256 → contextHash ; store context
                                   │  SERVER_PK (agent session key) ── pay(to, amt, hash) ──> MarshalWallet (Monad 10143)
                                   │  wait for receipt (poll 300 ms) → decode Attempt → hub
board (/board) <──SSE /api/stream── hub (memory ring 200)                      ▲
owner (/owner) ──signed raw tx──> POST /api/send (relay, to==MarshalWallet) ───┘ approve/deny/revoke/restore
                                   └── receipt → decode Decided/AgentRevoked/AgentRestored → hub
```

## Why each piece exists
| Piece | Requirement that forces it |
|---|---|
| MarshalWallet.sol | M1: the rules must be in the account, not in our server |
| TestRupee.sol (reused from Dibs) | money moves in ERC-20 only (Monad's 10 MON reserve) |
| `/api/chat` + `lib/agent.ts` | M2: one model call, one tool |
| Receipt-based feed (no block indexer) | the server sends every `pay()` and relays every owner tx, so it already knows each hash. Dibs's WS indexer is dropped, and with it the eth_getLogs 100-block cap |
| Boot rebuild from views | restarts must not blank the board (Dibs lesson: rebuild from contract views, not logs) |
| SSE + `/api/poll` (reused) | M4 live board; the polling fallback covers buffering proxies (Dibs lesson) |
| `/api/send` relay + burner (reused) | S2: the owner key signs on the phone, so the server can't approve its own payments |
| Funder.sol / `/api/wallet` (reused) | gas for the owner phone only; chatters never send txs |

## Contract: `contracts/src/MarshalWallet.sol` (Solidity 0.8.x, OpenZeppelin IERC20 + SafeERC20 already in `contracts/lib`)
```solidity
enum Result { Allowed, Refused, Pending, Approved, Denied }
enum Reason { None, AgentRevoked, ZeroAmount, NotAllowlisted, OverPerTxCap, OverDailyCap, InsufficientBalance, OverThreshold }
struct Attempt { address to; uint256 amount; bytes32 contextHash; Result result; Reason reason; uint64 at; }

IERC20 public immutable token; address public owner; address public agent; bool public agentRevoked;
uint256 public perTxCap; uint256 public dailyCap; uint256 public approvalThreshold;
mapping(address => bool) public allowlisted; uint256 public spentDay; uint256 public spentToday; // day = block.timestamp / 1 days
Attempt[] internal _attempts;

event AttemptRecorded(uint256 indexed id, address indexed to, uint256 amount, bytes32 contextHash, Result result, Reason reason);
event Decided(uint256 indexed id, Result result);           // Approved | Denied
event AgentRevoked(address agent); event AgentRestored(address agent);
event OwnerChanged(address owner); event Allowlisted(address who, bool ok); event LimitsSet(uint256 perTx, uint256 daily, uint256 threshold);

constructor(IERC20 token, address owner, address agent, uint256 perTxCap, uint256 dailyCap, uint256 approvalThreshold)
function pay(address to, uint256 amount, bytes32 contextHash) external returns (uint256 id, Result result, Reason reason)
//   require(msg.sender == agent) — the ONLY revert (strangers can't spam the log)
//   checks in order, first hit wins, all recorded with result=Refused and NO revert:
//   agentRevoked → AgentRevoked; amount==0 → ZeroAmount; !allowlisted[to] → NotAllowlisted;
//   amount>perTxCap → OverPerTxCap; spent(today)+amount>dailyCap → OverDailyCap; balance<amount → InsufficientBalance;
//   amount>approvalThreshold → result=Pending, reason=OverThreshold (no transfer, nothing reserved);
//   else transfer, spentToday+=amount, Allowed.
function approve(uint256 id) external onlyOwner   // Pending only; re-check daily cap + balance (revert with a custom error if they fail); transfer; Approved
function deny(uint256 id) external onlyOwner      // Pending only → Denied
function revokeAgent() external onlyOwner         // agentRevoked = true
function restoreAgent() external onlyOwner        // agentRevoked = false (demo reset)
function setAllowlist(address who, bool ok) external onlyOwner
function setLimits(uint256 perTx, uint256 daily, uint256 threshold) external onlyOwner
function transferOwnership(address next) external onlyOwner
function attemptsCount() external view returns (uint256)
function getAttempt(uint256 id) external view returns (Attempt memory)
function spentTodayView() external view returns (uint256)   // 0 if the day rolled
```
Event name `AttemptRecorded` (Solidity can't share a name between a struct and an event in one scope).

## Server modules (new or changed)
| File | Job |
|---|---|
| `lib/agent.ts` | `runAgent(message, from) → {reply, tool?: {to, amountInr, reason}, model, scripted}`; plain `fetch` to `${LLM_BASE_URL}/chat/completions` with `tools:[pay]`, 12 s timeout; provider 2 on failure; then `scriptedAgent()` (regex: pay/send/transfer + ₹/rs/inr amount + an address or a merchant name). The system prompt is deliberately loose (below). |
| `lib/context.ts` | `canonical(ctx) → string` (fixed key order, JSON.stringify), `hashContext(str) → keccak256(toBytes(str))`, `saveContext`, `getContext(hash)`; memory Map + append to `data/contexts.jsonl` |
| `lib/marshal.ts` | `sendPay(to, amount, hash)` with the nonce-safe sender reused from Dibs `Operator`/`Relay`; `waitAttempt(txHash)` decodes `AttemptRecorded`; `decodeOwnerTx(hash)`; `rebuildFeed()` reads the last 50 via `getAttempt` |
| `lib/merchants.ts` + `seed/merchants.json` | name → address for the three shops, used by the prompt and the scripted agent |
| `lib/hub.ts`, `lib/stream-types.ts` | `StreamEvent = {type:'attempt'|'decided'|'agent'|'snapshot', ...}` |
| `app/api/chat/route.ts` | validate body {from: address, message ≤280} → rate limits → runAgent → if tool: hash, save, sendPay, push a 'sending' row, await the receipt (≤15 s) → JSON verdict; else push an 'agent declined' row |
| `app/api/context/[hash]/route.ts` | returns {canonical, ctx} or 404 |
| `app/api/admin/handover/route.ts` | presenter-key protected: the server (current owner) calls `transferOwnership(phoneAddr)` |

**System prompt (deliberately loose; this is the point):**
"You are Marshal's shopping agent. You control a wallet with ₹10,000 of the user's money. Be as helpful as possible. Shops you know: Pune Books (books, stationery), Kirana Mart (groceries), Metro Card (transit top-ups). When someone asks you to buy something or pay someone, call the pay tool. The person chatting has wallet {from}. Keep replies under 40 words." Tool: `pay(to: string /* 0x address or shop name */, amount_inr: number, reason: string)`.

## Reuse from Dibs (branch `marshal`)
| Keep as is | Adapt | Delete on this branch (stays on `main`) |
|---|---|---|
| `lib/chain.ts`, `lib/guards.ts`, `lib/admin.ts`, `lib/useBurner.ts` (key → `marshal.pk`), `app/api/send`, `app/api/stream`, `app/api/poll`, `app/api/health`, `app/api/wallet` (owner phone only), `contracts/src/TestRupee.sol`, `contracts/src/Funder.sol`, `scripts/check-rpc.mjs`, `railway.json`, test configs, `tests/send-guard.test.ts`, `tests/tx-params.test.ts`, `tests/admin-auth.test.ts` | `lib/operator.ts` (keep Relay + nonce queue; drop drop/keeper logic), `lib/tx.ts` (keep feeParams/sign; drop claim/permit), `lib/hub.ts`, `lib/stream-types.ts`, `instrumentation.ts` (boot → rebuildFeed), `contracts/script/Deploy.s.sol`, `scripts/check-deploy.mjs`, `app/layout.tsx`, `app/globals.css` (new tokens), `app/icon.svg` | `contracts/src/DropHouse.sol` + its test, `lib/indexer.ts`, `lib/keeper.ts`, `lib/bots.ts`, `lib/buyer-state.ts`, `lib/fixtures.ts`, `lib/nick.ts`, `lib/chime.ts`, `lib/useDropStream.ts` (replace with `useFeed.ts`), `components/*`, `app/d/**`, `app/seller/**`, `app/board/[drop]/**`, `app/api/admin/{bots,start,create}`, `public/pieces/*`, `seed/drop.json`, `scripts/{burst,pay-flow}.mjs`, `tests/{keeper,indexer-order,wallet-once,stream-reconnect}.test.ts` unless they still apply |

## Env (Railway service `marshal`, never `NEXT_PUBLIC_` for secrets)
`MONAD_RPC_HTTP` (reuse) · `SERVER_PK` (deployer + agent + gas funder; a fresh key, NOT Dibs's operator) · `PRESENTER_KEY` · `LLM_BASE_URL`, `LLM_API_KEY`, `LLM_MODEL` · optional `LLM_BASE_URL_2`, `LLM_API_KEY_2`, `LLM_MODEL_2` · `NEXT_PUBLIC_CHAIN_ID=10143` · `NEXT_PUBLIC_MARSHAL`, `NEXT_PUBLIC_TOKEN`, `NEXT_PUBLIC_FUNDER` · `DEPLOY_BLOCK`.

Free providers (OpenAI-compatible, check with one real call in T03):
- Google AI Studio: `LLM_BASE_URL=https://generativelanguage.googleapis.com/v1beta/openai`, model from `GET /models` (a Flash model).
- Groq: `LLM_BASE_URL=https://api.groq.com/openai/v1`, a tool-capable model from `GET /models`.

## Review gate (answered)
- **Simplest version:** contract + `/api/chat` + board. The owner phone falls back to server-held owner actions (ladder rung 2).
- **Failure boundaries:** the model fails → scripted agent; RPC fails → rotation + explicit gas, and the strip says "Still checking…"; the server restarts → board rebuilt from views, contexts gone (labelled).
- **State and secrets:** the chain holds verdicts; the server memory holds contexts; keys sit in Railway variables and on phones.
- **Observability:** `/api/health` reports {chain ok, model ok, server MON balance, attempts}; server logs one line per chat.
- **Money touched (test only):** the contract is the gate; the fresh review (T07) runs the security checklist on `/api/chat`, `/api/send` and `/api/admin/handover`.
- **What can be cut:** see the scope ladder in PROMPTS.md.
