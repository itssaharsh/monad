# HANDOFF: Monad Blitz Pune V3 (read this first in any new session)

**State at 2026-10-10 ~01:30 IST:** prep only. No product code exists, and none may be written before the event starts (Blitz rule: no pre-built projects; coding starts in event hours).

## Decided
- **Idea:** **Parchi**, a live, verifiable allocation draw for housing-society parking. Everyone in the room joins by QR, adds randomness, gets a slot or a waitlist rank, and recomputes the result on their phone. → `docs/PROJECT.md`
- **UI:** `docs/UI-SPEC.md` (tokens, screens, states, copy) + `docs/design/mockup.html` (reference only, not to be copied into the product).
- **Build:** `docs/BUILD-PLAN.md`. Contract design, architecture, hour-by-hour plan, scope ladder, deploy steps.
- **Research:** `hackathon-idea/monad-blitz-pune-v3/` (`idea-package.md`, `judging-intel.md`, `past-submissions.md`, `discovery.md`).

## Key facts
- Monad testnet: chain 10143, RPC `https://testnet-rpc.monad.xyz`, faucet `faucet.monad.xyz`. Gas is charged on the gas **limit**. The public RPC is rate-limited.
- Judging (from a London participant's copy of the shared Blitz brief): 3-minute live demo; builders vote on phones during demos and for 15 minutes after; criteria are novelty, innovative mechanics using Monad, consumer problem-solving, learning/experimentation. Must have a fork of the event repo, a README, contracts deployed during the event, a public repo and a live URL ("locally hosted projects will be disqualified").
- Toolchain checked from npm: viem 2.57, next 16.4, tailwind 4.3, solc 0.8.37, @foundry-rs/forge 1.7.1, qrcode.react 4.2.
- Skills: the user's plugins are unzipped under the session scratchpad (they're lost if the container resets). Re-upload them, or use the built-in `anthropic-skills:product-demo-video` for the video.

## Blocked tonight → needs the user
1. Network access → **Full** (Monad RPC, faucet, Vercel API, GitHub releases, Google Fonts were blocked or uncertain).
2. Env var `MONAD_DEPLOYER_KEY` (fresh burner, ~2 testnet MON).
3. The event's submission repo link (or Blitz Portal).

## First commands at "go"
1. Probe: chainId, deployer balance, `blockhash(block.number-1)` non-zero on testnet.
2. Fork the event repo → `add_repo` (push access) → work there; `pb.py init --mode hackathon --channel direct`.
3. Follow `docs/BUILD-PLAN.md` §3 from T00.
