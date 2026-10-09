# Parchi: build plan for Monad Blitz Pune V3 (prod-build, hackathon mode, direct channel)

Written the night before. **No product code exists before the event.** At "go", this plan becomes `.prod-build/contract.md` + `plan.json` in the event fork (`pb.py init --mode hackathon --channel direct`).

## 1. Implementation contract

**Intent.** A committee runs a parking draw at an AGM. Residents join from their phones with no wallet, add randomness, and get a slot or a waitlist rank that anyone can recompute.

| MUST (demo path) | SHOULD | WON'T today |
|---|---|---|
| Create draw (title, slot count, eligible flats) with list hash frozen on-chain | `?state=` for every board/phone state | flat ownership checks / KYC |
| QR join: pick unclaimed flat, commit secret (gasless, signed) | replay (alt+shift+p) of a recorded run | priority rules (EV, senior) |
| Close entries (host-signed) | per-IP rate limit on relayer | proxies for absent members |
| Auto-reveal from phones; host ends reveals | waitlist view | mainnet, real money |
| Draw: seed from reveals + block hash after reveal close; allocation as a deterministic view | OG image + favicon | accounts, history pages |
| Re-run reverts on-chain and the board shows it | | |
| Phone result (slot or waitlist #) + Verify page that recomputes everything in the browser | | |
| "Add N simulated residents" (labelled) for rehearsal and as a live fallback | | |
| Deployed on Vercel + Monad testnet; README; fork + PR | | |

**Core journeys:** J1 host create → board · J2 resident join → result → verify · J3 host close → draw → re-run attempt.

**States:** board `open · frozen · revealing · sealing · drawn · rerun-reverted · error`; phone `pick-flat · dropping · in-bowl · revealing · won · waitlist · closed-late · error(offline: retry from localStorage)`; verify `computing · matches · mismatch · error`.

**Acceptance criteria (each has a check):**
1. WHEN a flat already joined, THE SYSTEM SHALL revert `FlatTaken` (forge test).
2. WHEN entries are closed, THE SYSTEM SHALL revert any join with `EntriesClosed` (forge test; phone shows closed-late).
3. WHEN a draw is drawn, THE SYSTEM SHALL revert any second draw with `AlreadyDrawn` (forge test; live demo).
4. WHEN a reveal doesn't match its commit, THE SYSTEM SHALL revert `BadReveal` (forge test).
5. WHEN a join signature isn't from the claimed resident, THE SYSTEM SHALL revert `BadSig` (forge test).
6. THE allocation view SHALL equal the browser verifier's output for the same draw (parity test: 3 seeds × n ∈ {1, 7, 41}).
7. WHEN 40 simulated residents join, THE SYSTEM SHALL land all entries within 20 s on testnet (timed script, evidence logged).
8. THE deployed URL SHALL pass the full demo flow 3× in a row with a reset between runs (observed, logged).

**Constraints.** Monad testnet (chain 10143, `https://testnet-rpc.monad.xyz`, faucet `faucet.monad.xyz`). **Gas is charged on the gas LIMIT**: every tx sets an explicit limit = measured × 1.25. Public RPC is rate-limited: browsers never call it directly. Relayer endpoint is public: abuse limits (per-IP rate, max 300 entries per draw, signatures checked before sending). Zero AI spend. Freeze about 5:45 PM (assumed from past Blitz timelines).

## 2. Architecture

```
phones / board (Next.js pages, viem burner keys in localStorage)
   │  POST signed actions                    GET state (polled 0.7–1.5 s)
   ▼                                            ▼
/api/relay  (Node route, Fluid compute)     /api/draw/[id]  (multicall, Cache-Control s-maxage=1)
   │  K=6 relayer keys, per-key nonce queue     │
   └────────────► Parchi.sol on Monad testnet ◄─┘
                         ▲
/api/sim (server-made burner residents, labelled "Simulated residents")
```

| Component | Why it exists |
|---|---|
| `Parchi.sol` | the frozen list, the commit/reveal record and the seed must be public and unchangeable; that is the product |
| `/api/relay` | residents have no wallet or MON; the relayer pays gas, but can't forge entries (EIP-712 signatures checked on-chain) |
| `/api/draw/[id]` | one cached multicall per second instead of 100 phones hammering the rate-limited public RPC |
| `/api/sim` | rehearsal, the video, and the fallback if venue Wi-Fi fails |
| Next.js on Vercel | route handlers + dynamic routes in one deployable |

**Contract (Solidity ^0.8.24, OpenZeppelin ECDSA + EIP712).**
- `createDraw(string title, uint32 slots, bytes32[] flats, address host)` → id. Stores flats (labels packed in bytes32), `listHash = keccak256(abi.encode(flats))`, host. Relayer-submitted; `host` is the host burner address. Emits `DrawCreated`.
- `joinFor(uint id, uint32 flatIdx, bytes32 commit, address resident, bytes sig)`: EIP-712 `Join(id,flatIdx,commit)` signed by `resident`; phase Open; flat unclaimed; one flat per resident. Records join order. Emits `Joined(id, flatIdx, resident, block)`.
- `closeEntries(uint id, bytes hostSig)`: EIP-712 `Close(id)`; phase Open→Revealing; `entryCount` frozen. Emits `EntriesClosed(id, entryCount, block)`.
- `reveal(uint id, uint32 flatIdx, bytes32 secret)`: no signature needed: `keccak256(abi.encode(secret, id, flatIdx, resident)) == commit`; `revealAcc ^= keccak256(abi.encode(secret))`. Phase Revealing.
- `endReveals(uint id, bytes hostSig)`: phase Revealing→Sealing; `sealBlock = block.number`.
- `draw(uint id)`: permissionless; requires `block.number > sealBlock + 1`; `seed = keccak256(abi.encode(revealAcc, blockhash(sealBlock + 1), id))`; phase → Drawn. Second call reverts `AlreadyDrawn`. Emits `Drawn(id, seed)`.
- `allocation(uint id) view returns (uint32[] order)`: Fisher–Yates over join order with `j = uint(keccak256(abi.encode(seed, i))) % (i + 1)`, iterating i = n−1…1. First `slots` entries get P-01…; the rest are the waitlist in order.
- Views: `getDraw`, `getFlats`, `getEntries` (flatIdx, resident, commit, revealed).
- **Why the last revealer can't bias it:** anyone choosing to withhold their reveal decides before `sealBlock + 1` exists, so they can't compute either outcome. Withheld entries stay in the draw. (Residual: the block producer; disclosed as a limit.)
- **Spike in hour 1:** confirm `blockhash(n-1)` is non-zero on Monad testnet. Fallback: `block.prevrandao` of the `draw` tx's block, same withholding argument.

**Relayer.** Keys `k_i = keccak256(DEPLOYER_KEY ‖ i)`, i = 0..5, funded with 0.3 MON each by `scripts/fund.ts`. Pick by `hash(resident) % 6` (host actions → key 0). Per-key in-memory promise queue + nonce cache, re-synced from `getTransactionCount(pending)` on any nonce error (max 3 retries). Explicit gas limits per function. Verify the signature off-chain first (cheap reject). Rate limit: 30 req/min per IP, 300 entries per draw.

**Verifier (browser).** Fetch flats, entries, commits, reveals (from `/api/draw/[id]?full=1`), `getBlock(sealBlock+1).hash` via a read route. Recompute listHash → revealAcc → seed → Fisher–Yates; compare with `allocation(id)`. The JS shuffle uses the same `keccak256(abi.encode(seed, i))` via viem `encodeAbiParameters`. Parity test in CI.

**Review-gate answers.** Necessity: every component is on the demo path. Failure: RPC down → board shows last state + "Chain is slow, retrying"; phone keeps its entry in localStorage and retries; Wi-Fi down → simulated residents keep the demo alive. Secrets: deployer key only in Vercel env (sensitive) and the session env var; burner keys only in each browser. Observability: relayer logs tx hash + latency per action to Vercel logs. Cut list if late: SHOULD column, then waitlist screen, then the motion on slot fill.

## 3. Hour-by-hour (H0 = "go", ~10:00 IST after workshops; freeze ~17:45)

| When | Task | Done when (evidence) |
|---|---|---|
| H0:00–0:20 | **T00 setup**: fork the event repo → `add_repo`; scaffold Next 16 + Tailwind 4; Foundry (`foundryup`; fallback `npm i @foundry-rs/forge`); chain probe (chainId 10143, deployer balance, blockhash spike) | probe script prints chainId + balance + non-zero blockhash |
| H0:20–1:30 | **T01 contract + forge tests** (AC 1–5) → deploy, verify on explorer, fund relayer keys | `forge test` green; contract address + tx links in README |
| H1:30–2:15 | **T02 relayer + reads + sim routes**; script: 40 sim residents full flow on testnet, timed | AC7 log; **first Vercel deploy** by H2:30 |
| ⏸ check-in 1 (2 min) | You: open the URL on your phone, see the board | — |
| H2:15–4:15 | **T03 UI**: tokens/fonts → S4 phone → S3 board → S2 create (pre-filled "Blitz Heights CHS") → S5 verify → S1 landing | golden path on the deployed URL; AC6 parity test green |
| H4:15–4:55 | **T04 polish**: drop/fly/unfold/stamp motion, `?state=`, reset/replay shortcuts, 390/1440/1920 pass, favicon/OG | screenshots at 3 viewports |
| H4:55–5:30 | **T05 gates**: fresh-eyes review subagent (bugs + security), fix blockers; demo flow ×3 on prod with 40 sim + 3 real phones | AC8 logged; DELIVERY.md |
| ⏸ check-in 2 (5 min) | You + 2 friends scan the QR: real-phone test | — |
| ~17:30 | **Freeze.** README final, PR to the event repo, submission form | PR link |
| 17:30–18:30 | Video via product-demo-video (runs while you rehearse) · you rehearse the pitch 3× | video file |

**Scope ladder if behind:** (1) drop S1 landing (start on `/new`), (2) drop waitlist + replay, (3) drop motion except the slot fill, (4) commit-reveal → single-step joins with block-hash seed (keep the frozen list + verify).

## 4. Deploy

- Contract: `forge create` (or viem script) with `MONAD_DEPLOYER_KEY`; explicit `--gas-limit`; save address in `NEXT_PUBLIC_PARCHI_ADDRESS`.
- Web: Vercel project from the fork (Vercel MCP `create_project`/git import; fallback `vercel deploy --prod` with a token if the git import needs approval). Env: `RELAYER_ROOT_KEY` (sensitive), `NEXT_PUBLIC_PARCHI_ADDRESS`, `MONAD_RPC_URL`.
- After every deploy: smoke `/`, `/new`, `/api/draw/1`; phone pass at 390; warm the URL 5 min before pitching.

## 5. Morning prerequisites (you, before 9:30)

1. Network access = **Full** on this cloud environment (Monad RPC, faucet, Vercel API, GitHub releases are blocked tonight).
2. Environment variable **`MONAD_DEPLOYER_KEY`** = private key of a **fresh burner** wallet with ~2 testnet MON (faucet.monad.xyz, or ask the organisers). Never your main wallet.
3. If the settings don't reach this session, open a **new** cloud session on `itssaharsh/monad` and say: "Continue from HANDOFF.md".
4. When the organisers share the event repo (or Blitz Portal), paste the link here.
