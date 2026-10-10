# Implementation contract: Marshal (hackathon mode, relay channel)

## 1. Intent
- **Who:** anyone who lets an AI agent hold money. Tonight that's the room at Monad Blitz Pune, playing the attacker, plus the presenter as the owner.
- **What they can do:** chat with a deliberately gullible shopping agent that holds ₹10,000 in test rupees, and watch the wallet contract decide.
- **What changes after:** the agent can be talked into anything, but money only leaves through the wallet's rules. Every refusal is on-chain with a hash of the chat that caused it, and the owner can approve, deny or revoke from a phone.

## 2. Scope
- **MUST:**
  - M1 MarshalWallet.sol, with tests for every rule.
  - M2 The agent server: one model call, one `pay` tool, the context hash, sending `pay()`, the stored context, rate limits.
  - M3 The phone chat.
  - M4 The projector board: the live feed, a balance that holds, and replay.
  - M5 Owner Approve / Deny / Revoke.
  - M6 Deployed on Railway, and the submission.
- **SHOULD:**
  - S1 The browser re-checks the replayed context's hash against the chain.
  - S2 The owner key lives only on the owner's phone (it signs; the server relays).
  - S3 The phone shows the payout wallet's tINR balance.
- **COULD (listed, not built):** setting limits from the UI, adding payees from the UI, auto-revoke after N refusals, multiple agents.
- **WON'T now (roadmap):** tx simulation, behavioural baselines, ERC-4337 / 7579 modules, OpenTelemetry, mainnet, real money, EIP-7702.

## 3. Core journeys
- **J1 Attack:** the phone sends a message → the server calls the model → the model calls `pay(to, amount, reason)` → the server hashes the context and sends `MarshalWallet.pay()` → the contract records and emits `Attempt` (it does not revert) → the board shows a REFUSED strip with the reason, and the balance is unchanged.
- **J2 Legit buy:** the same path with an allowlisted shop under ₹1,000 → ALLOWED → the balance drops.
- **J3 Owner:** a shop payment between ₹1,000 and ₹3,000 → WAITING → the owner phone shows it and taps Approve (the tx is signed on the phone and relayed) → APPROVED on the board. Revoke agent → the next attempt shows REFUSED · Agent revoked.
- **J4 Replay:** click a strip → the drawer shows the stored context, and the browser recomputes keccak256 and matches it with the on-chain `contextHash`.

## 4. Surfaces
`/` phone chat (S1) · `/board` (S2 + S2a drawer) · `/owner?key=` (S3) · `POST /api/chat` · `GET /api/context/[hash]` · `GET /api/stream` (SSE) + `GET /api/poll` (fallback) · `POST /api/send` (relay, owner phone) · `POST /api/wallet` (gas for the owner phone) · `POST /api/admin/handover` · `GET /api/health`.

## 5. States per journey
| State | J1/J2 phone | J1/J2 board | J3 owner | J4 replay |
|---|---|---|---|---|
| initial | "Making your payout wallet…" | empty + big QR | locked without key | n/a: opens on click |
| loading | "Asking the agent…" | strip "Checking with the wallet…" | "Approving…" | "Loading recording…" |
| success | VerdictCard | stamped strip | card → Decided | context + "Hash matches ✓" |
| error | "That didn't reach the agent. Send again." | strip "Still checking… (tx ↗)" after 15 s | "Didn't go through: <reason>. Try again" | "No recording kept for this one" |
| empty | 3 suggestion chips | "Waiting for the first message" | "Nothing waiting" | n/a |
| partial | agent replied without a tool call → "The agent didn't try to pay" | grey strip | n/a | context without tx (agent declined) |
| retry | Send again | SSE reconnect, then poll | Try again | Close and reopen |
| disabled | Send disabled while sending, during the rate limit, and over 280 chars | n/a | buttons disabled while a tx is pending | n/a |
| permission-denied | rate limited: "One message every 15 seconds" | n/a | no key → "This page needs the owner link" | n/a |
| offline/degraded | model down → labelled scripted agent | "Reconnecting…" | relay down → error + retry | hash mismatch shown in red |

## 6. Data
| Entity | Key fields | Owner | Persistence | Source of truth |
|---|---|---|---|---|
| Attempt | id, to, amount, contextHash, result, reason, at | MarshalWallet | chain | chain |
| Context | hash, canonical JSON string {v, chatId, from, message, reply, tool{to,amount,reason}, model, scripted, at} | server | memory map + `data/contexts.jsonl` (lost on redeploy, which is acceptable) | the server; the chain holds its hash |
| Feed row | attemptId, chatId, message preview, verdict, txHash | server | memory ring of 200; rebuilt from contract views at boot without messages | chain for verdicts |
| Burner | private key | each phone | localStorage `marshal.pk` | phone |
| Owner key | private key | owner phone | localStorage `marshal.owner.pk` | phone |

## 7. Interfaces
| Boundary | Contract | Failure | Fallback |
|---|---|---|---|
| Model (OpenAI-compatible `/chat/completions`, free tier) | `lib/agent.ts#runAgent(msg, from) → {reply, tool?, model, scripted}` | 429 / timeout 12 s / malformed tool args | next provider in `LLM_*_2`, then the scripted agent (regex for "pay/send ₹N to X"), labelled `scripted: true` |
| Monad RPC | `lib/chain.ts` rotation (reused) | 429, lag | rotate URLs; explicit gas; receipt polling every 300 ms up to 15 s |
| MarshalWallet | ABI in `lib/abi/MarshalWallet.json` | revert on an owner action | humanized reason on the owner phone |
| SSE | `lib/stream-types.ts#StreamEvent` | a proxy buffers it | `/api/poll` after 4 s (reused) |
| Relay `/api/send` | raw signed tx; `to` must be MarshalWallet | bad tx | 400 with a reason (reused guard) |

## 8. Constraints
- Chain 10143. Gas is billed on the limit: `pay` gas 300k, owner actions 200k, set explicitly. Min base fee 100 gwei. Keep the server key above 12 MON (the reserve is 10). Money moves in tINR only.
- One process holds `SERVER_PK` (nonce safety): one Railway replica, and no laptop server while Railway runs.
- **Abuse limits:** `/api/chat` allows 1 message per 15 s per payout address and 6 per minute per IP, ≤280 chars, ≤2 concurrent model calls (the rest queue, max 10, then 503 "busy"). `/api/send` allows only MarshalWallet as `to` (5 per 10 s per IP, reused). `/api/wallet` funds the owner phone once.
- Secrets live only in Railway variables and `.env`, never in `NEXT_PUBLIC_*`.
- Judging criteria are not published for this event ("Peer-Judged: Your fellow builders choose the winners"). We plan against: technical execution, usefulness, originality, design, use of Monad.

## 9. Acceptance criteria
- AC-1 WHEN the agent calls pay to a non-allowlisted address THE SYSTEM SHALL record Attempt(result=Refused, reason=NotAllowlisted), not revert, and not move tokens. Verify: `forge test --match-test test_refuse_notAllowlisted`
- AC-2 WHEN amount > perTxCap / spent+amount > dailyCap / amount > balance / amount == 0 THE SYSTEM SHALL record Refused with OverPerTxCap / OverDailyCap / InsufficientBalance / ZeroAmount. Verify: forge tests per reason
- AC-3 WHEN an allowlisted amount is ≤ approvalThreshold THE SYSTEM SHALL transfer and record Allowed; WHEN threshold < amount ≤ perTxCap THE SYSTEM SHALL record Pending and move nothing. Verify: forge
- AC-4 WHEN the owner approves a Pending id THE SYSTEM SHALL re-check the daily cap and balance, transfer, and emit Decided(Approved); deny emits Decided(Denied); non-owner calls revert. Verify: forge
- AC-5 WHEN the owner revokes THE SYSTEM SHALL record every later agent pay as Refused(AgentRevoked) without transfer; `restoreAgent` re-enables it. A caller that isn't the agent reverts (no log spam). Verify: forge
- AC-6 WHEN `/api/chat` gets a message whose model reply has a pay tool call THE SYSTEM SHALL send `pay()` with keccak256(canonical context) and return {attemptId, verdict, reason, txHash}. Verify: `npm test -- agent` (mocked model + chain) and the live observe item
- AC-7 IF the model fails or times out THEN THE SYSTEM SHALL answer with the scripted agent and mark the row `scripted`. Verify: `npm test -- agent`
- AC-8 IF the same payout address sends twice within 15 s THEN THE SYSTEM SHALL return 429 with the seconds left. Verify: `npm test -- guards`
- AC-9 WHEN an Attempt or Decided event is mined THE SYSTEM SHALL push it to every board within 2 s. Verify: observe on the live URL
- AC-10 WHEN a strip is opened THE SYSTEM SHALL show the stored context and "Hash matches the chain ✓" after recomputing keccak256 in the browser. Verify: observe
- AC-11 WHEN the board opens with no input THE SYSTEM SHALL lead with the wallet balance, the feed of refused or allowed strips, and the QR. Verify: observe at 1440×900
- AC-12 WHEN the owner taps Approve / Deny / Revoke THE SYSTEM SHALL sign on the phone, relay, and update the board within 3 s. Verify: observe

## 10. Assumptions
- A-1 [safe] The free model tier holds up for a room of ~40 under 1 message per 15 s per phone. If not, the scripted fallback (AC-7) keeps verdicts landing.
- A-2 [reversible] The SERVER key is deployer + agent session key + gas funder; ownership is handed to the owner phone after deploy. The fallback (scope ladder rung 2) keeps the server as owner behind `PRESENTER_KEY`.
- A-3 [reversible] Merchants are three derived addresses with names in `seed/merchants.json`; no private keys are needed.
- A-4 [reversible] Limits: balance ₹10,000 · perTxCap ₹3,000 · dailyCap ₹8,000 · approvalThreshold ₹1,000.
- A-5 [safe] Contexts are lost on redeploy. The board then says "No recording kept for this one".

## 11. Demo script
See `/marshal/PITCH.md` (2:30). Build only what appears in it.

## 12. Event and judged path
- **Window:** start 2026-10-10 12:00 IST (06:30Z, assumed from the Mumbai V3 template: "start assumed 12:00 IST (Mumbai template), not announced to us"), freeze 17:00 IST (11:30Z), end / code freeze 17:30 IST (12:00Z, "user 13:44 IST: stop time 17:30 IST"; confirmed again at 15:25 IST). Source: `.prod-build/event.json` (provisional). Pitches ~18:45 IST, peer vote ~20:30 IST. Rule on earlier work, from the event page: "Come with an idea"; all code is written today.
- **How an entry is judged:** a live pitch to the room, then builders vote ("Peer-Judged: Your fellow builders choose the winners"). Submission is a fork of monad-developers/monad-blitz-pune ("Give it your project name, a one-liner description, make sure you are forking `main` branch") plus the Blitz Portal (blitz.devnads.com). A London participant's copy of the shared Blitz brief mentions a 3-minute demo, a live URL ("locally hosted projects will be disqualified") and contracts deployed during the event. Not re-confirmed for Pune.
- **Criteria:** not published for Pune. Planning set: technical execution, usefulness, originality, design/presentation, use of Monad.
- **Criteria coverage:**
  - technical execution: the contract's refusal-without-revert plus the live tx feed (met when T05 passes);
  - usefulness: the Freysa / AIXBT losses, bounded by the wallet's caps (pitch);
  - originality: refusals you can replay, with hash proof (T04, S1);
  - design: flight-strip board and stamps (T04);
  - use of Monad: every attempt, refused ones included, is a real tx that lands on the board in ~1 s. Recording refusals on-chain is only sensible because the fees are low and blocks are 300 ms (T03/T04).
- **Capability coverage:** no sponsor prize capabilities published. Monad testnet contract deployed during the event (T02).
- **Main result:** a camera sees the projector, where the room's attacks land as red REFUSED stamps while the ₹10,000 balance holds, on live Monad testnet txs. The staged failure is the owner pressing Revoke, after which the next attempt reads AGENT REVOKED.
- **Promise coverage:** "kill switch" → the Revoke button on the owner phone and the board's status chip · "audit trail" → the feed plus the replay drawer · "AI agents that hold crypto wallets" → the agent's reply on each strip and the wallet balance readout. All three are on the board's first screen at 1440×900 (AC-11).
- **Before the clock:** the Marshal idea doc (Sept 2026, no code) and this plan (written 15:20–15:45 IST today, after the window opened). The Dibs code in this fork was written today 12:56–14:52 IST and parts of it are reused. README sentence: "Marshal's idea doc predates the event (Sept 2026, no code). All code was written on 10 Oct 2026 during Monad Blitz Pune; this fork first held Dibs, built earlier the same day, whose wallet, relay and live-stream code Marshal reuses."
