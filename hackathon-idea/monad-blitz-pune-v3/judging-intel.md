# Judging intelligence: Monad Blitz Pune V3 (Sat 10 Oct 2026)

Subagent J · researched 2026-10-09 · feeds idea selection and demo design. Raw submission list: [`past-submissions.md`](./past-submissions.md).

**How to read the sources.** Three kinds of source appear below, and the label says which:
- **opened (git):** files I read myself from public `monad-developers/monad-blitz-*` repos via `git fetch` of PR refs. Quotes from these are verbatim.
- **search excerpt (not opened):** text the web-search tool returned from a page I could not open (Notion, Luma, docs.monad.xyz and X are blocked from this container). Treat it as a lead with medium confidence, never as a verbatim quote.
- **user-relayed:** from the Luma page the user pasted (see `brief.md`).

The single most useful source turned out to be a participant's transcription of the official organizer brief, committed to a public PR: `monad-developers/monad-blitz-london` PR #2, file `docs/blitz-rules.md` (opened (git); written 8 Aug 2026 by the PodShop Arena team, "merged from the official 60-slide hackathon brief … and the organisers' written rules / Submission Process"). It's secondary (a participant's copy) but specific, recent and consistent with independent sources, so I use it with medium-high confidence and flag where it's London-specific.

---

## 1. Official criteria & rules

| # | Rule or criterion (quoted) | Source · confidence | Implication for Pune V3 |
|---|---|---|---|
| R1 | "Zero Constraints: Build any consumer app on Monad. If it runs, it's fair game." | Pune V3 Luma, user-relayed · high | Theme is open. Agent-economy themes (Pune V2, Mumbai V3, Bangalore V4) are gone this time; an agent-only idea is allowed but is no longer "on theme". |
| R2 | "Peer-Judged: Your fellow builders choose the winners." | Pune V3 Luma, user-relayed · high | Primary audience is other builders. See R7 for a possible judge half. |
| R3 | "Teams of up to 3 members allowed." | Pune V3 Luma, user-relayed · high (London allowed 4, Ankara 3; per-event) | Solo is fine. |
| R4 | New work only: no existing projects, no forks of existing codebases beyond standard libraries/boilerplates, no continuing personal projects; coding starts during event hours; research and planning beforehand are encouraged. | Monad Foundation "Rules & Guidelines (IMPORTANT PLEASE READ)" and Ankara info pack, search excerpt · medium; repeated in London transcription ("All project code must be written today … Planning ahead is encouraged") · medium-high | Arrive with the plan, design, copy and asset list; write no code before 9 AM. Templates and boilerplates (Scaffold-ETH 2, Monad templates) are allowed. Two past PRs openly say "Imported codebase from old repo" (Bangalore #27) and "moved code from original repo" (Hyderabad #8); peers can see commit history, so don't. |
| R5 | "Your project must be deployed and operational on the Monad Testnet for demonstration and review." | Rules & Guidelines (Ankara), search excerpt · medium | Contracts on testnet (chain 10143). London's brief allowed "Testnet or Mainnet"; testnet satisfies both. |
| R6 | Five eligibility gates: "Fork the starter repo", "Ship a proper README", "Deploy on Monad", "Deploy during the Blitz. Contracts must be deployed within the event timeline", "Live on the web. A working, deployed app — not slides alone." Also: "Locally hosted projects will be disqualified." and "Code lives in a public GitHub repo". | London brief transcription, opened (git) · medium-high (London, Aug 2026; the same Notion rules are shared across Blitz cities) | Budget the last hour for: fork of the Pune repo, README, Vercel deploy, contract deployed that day (tx hash visible). A demo on localhost may not be allowed to pitch. |
| R7 | "Results are decided by a mix of judge and participant votes, weighted 50% each." The same transcription flags a conflict: one slide says "No official judges. The room decides." | London transcription, opened (git) · medium. Independently, the Bangalore V4 team's playbook (June 2026) says "3-minute demo, judged 50% peer vote / 50% jury, rewarding **novelty + Monad-specific leverage** over polish" (`monad-blitz-bangalore` PR #42, `docs/monad_hackathon_playbook.md`, opened (git)) · medium. Pune V2's Luma agenda had a "Dinner, Judging & Community Vote" slot (search excerpt) · medium. Chinese editions: votes "by official judges and developers jointly" (search excerpt of event announcements) · medium. | Plan for both a peer room and a small judge panel (likely Monad DevRel/hosts). Pune V3's Luma says peer-judged only; treat 50/50 as possible, not confirmed. |
| R8 | Voting: "once the project presentations begin, all participating teams will gain access to a designated voting platform"; voting stays open during presentations "and for an additional 15 minutes after the final demo"; "teams will not be able to vote for their own project"; scores are tallied automatically. | Monad Foundation "Judging Process & Criteria" Notion page, search excerpt · medium; London transcription matches ("During presentations + 15 min after the final demo", portal `blitz.devnads.com` for "tokens, submission, voting") · medium-high | Voters vote on their phones while demos run. A demo that also asks phones to do something is competing with, and adjacent to, the voting portal. Ballot shape (one vote, top-N, or scores) is **unknown**. |
| R9 | Voting criteria: "Novelty & originality", "Innovative mechanics — … especially ones that leverage Monad's potential", "Problem-solving — does it creatively address a real or interesting challenge for consumer applications?", "Learning & experimentation". "Not the most polished or complete application — the new idea, the unique approach, the thing everyone can learn from." | London transcription, opened (git) · medium-high (stated as the organizer brief's criteria) | Novel mechanic + visible Monad leverage + a consumer problem. Feature count and polish are explicitly not the axis. |
| R10 | Demo: "Demo length | 3 minutes"; "Pitching order is determined by submission order"; "Slides are optional. The demo is the point."; "Your audience is everyone in the room — fellow builders, not VCs or non-technical judges. Impress developers." | London transcription, opened (git) · medium-high. Bangalore V4 playbook also says 3 minutes; Paris PR #2 `LISEZ-MOI.txt` mentions "le pitch de 3 minutes" · medium-high (3 independent teams) | One core moment inside 3 minutes. Submitting early fixes an early slot; submitting late gives a late slot (fresher at the 15-minute vote close but the room is tired). The Pune V3 order rule isn't confirmed. |
| R11 | Organizer presentation tips ("BONUS · VERY, VERY IMPORTANT"): "Slides — fewer slides, more demo"; "UI — don't ship vibe-coded UI. Give the LLM a brand kit … 'A real design beats a default Tailwind blob.'"; "Demo flow — pre-fill your forms, stage your demo. The judge sees the magic, not the typing." Written rules add: practise timing, show the core innovation only, prepare "screenshots of key flows, a short fallback recording, and a stable deployment." | London transcription, opened (git) · medium-high | Pick one coherent visual identity early; pre-seed state; record a 30-second fallback video before the freeze. |
| R12 | "We strongly discourage building direct clones of existing applications without introducing significant innovation or a unique Monad-specific twist." | Rules & Guidelines, search excerpt · medium; London: "What new problem am I solving, or how am I solving an old problem in a radically new way on Monad?" · medium-high | "X but on-chain" ideas need a reason Monad's speed makes them newly possible. |
| R13 | Timeline pattern: code freeze ~5:30–6:00 PM, submission deadline 15–45 min later, then pitches (Mumbai: "6:00 PM: Code Freeze; 6:15 PM: Submission Deadline; 6:45 PM: Pitches"; Ankara 6:45 PM deadline; London freeze 5:45 PM; Bangalore V4 freeze 17:15). | Luma pages and info packs, search excerpts; London and Bangalore transcriptions, opened (git) · medium | Assume a ~5:45 PM freeze for Pune V3 (assumption) and plan a feature freeze at ~4:30. |
| R14 | Prizes: $500 / $400 / $300 / $200 / $100. No sponsor tracks or bounties are mentioned for Pune V3. | user-relayed · high | One overall ranking. Nothing to gain from integrating a sponsor tool for its own sake. |

---

## 2. Format → demo implications

1. **Three minutes, live, in front of builders who vote on their phones during the demos** (R8, R10). The demo must land its one idea in the first ~60 seconds and be memorable at vote time, up to ~15 minutes after the last pitch. A one-line hook the room can repeat helps when voters scroll the portal list. (inference from R8/R10 · low-medium)
2. **The room is developers, maybe plus a judge half** (R7, R10). The brief says "Impress developers", and the criteria reward "innovative mechanics … that leverage Monad's potential". Builders discount a generic CRUD dApp with a wallet button; a mechanic that only works because blocks are ~0.3–0.6 s (R9, §7) reads as on-criteria. (organizer-stated · medium-high)
3. **Audience participation is rare in past entries.** Only 4 of 139 catalogued projects were explicitly built for the people in the room to join live (State Clash, BlitzBoard, Immortal, BRIC À BRAC), and 16 of 139 had any real-time shared state (counted in `past-submissions.md`). Whether those won is **unknown** (no winners found, §3). The room's phones are already out for voting (R8), so a QR join costs voters little. Those that did it removed wallet friction: State Clash used a Scaffold-ETH burner wallet so users "paint instantly without signing a single MetaMask popup"; BRIC À BRAC buyers join via QR at `/join/CODE` and the seller's phone needs no wallet; Proof Go used Privy email login (all opened (git)). (observed pattern · medium; payoff untested)
4. **Live and deployed or you may not pitch** (R6). Vercel or another public URL, contract deployed that day, README, fork of the event repo. A recorded fallback is encouraged (R11).
5. **Polish versus novelty.** The organizers both downweight polish ("Worry less about … Perfect UI / UX") and warn against unstyled AI-generated UI (R11). Read together: one consistent brand kit, no extra features. (organizer-stated · medium-high)
6. **Crowd load is a real risk.** Public RPC access is rate-limited (main.net network reference, search excerpt · medium), and Monad charges gas on the gas *limit*, not gas used (§7). A 50-phone demo should relay or batch transactions server-side, or use a provider RPC key, and set explicit small gas limits on sponsored wallets. (inference · medium)
7. **What wins peer votes elsewhere** (Reference B in the skill, not Blitz-specific): problem then a working demo within ~90 s; one "this is possible now" moment beats a feature tour. No "how I won Monad Blitz" write-up was found in 9 winner-focused searches.

---

## 3. Historical evidence: winners

**No winner of any Monad Blitz edition could be identified.** 11 searches (English, Chinese; city names, project names, "1st place", "runner up", LinkedIn, X) returned event listings only. The Blitz repos don't mark winners (no "winner" claims in any PR's Markdown, checked by `git grep` across all 17 repos). X, LinkedIn, Luma and the Blitz Portal are not readable from here.

| Event | Place | Project | What it did | Demo characteristics |
|---|---|---|---|---|
| Pune V1 (Dec 2025) | not found | – | – | – |
| Pune V2 "Agent Economy" (Jul 2026) | not found | – | – | – |
| Mumbai V2 / V3, Bangalore V1–V5, New Delhi, Hyderabad V1–V2, Nagpur, Bhopal, Lucknow | not found | – | – | – |
| Shenzhen (Jun 2025): winners "decided by audience voting" (TechFlow, search excerpt) | not found | – | – | – |
| Seoul 1st–3rd, Lagos, Denver, SF, London, Paris | not found | – | – | – |

**Base rates for winners: not computable (0 known winners).** The tables in §4 describe submissions, not winners. To close this gap, the user could check the Pune V2 Luma page's photos/updates, Monad India's X account, or ask in the event Telegram/Discord on the day which projects won V1/V2.

**Pune's own history (submissions, opened (git)):** V1 (13 Dec 2025): CrackPay (group bill-splitting mobile app), MonoKen (NFT event ticketing), Monad Arcade (stake-to-play games vs an AI bot, Farcaster mini app). V2 (4–5 Jul 2026, agent theme): Oracle (staked uptime-monitoring network), PiggyBag (AI agent that grants MON to builders). Only these 5 reached the repo; most V2 entries went through the Blitz Portal instead (see §5).

---

## 4. Saturation (submissions by category)

Counts from 139 projects in 17 repos (method and per-project rows in `past-submissions.md`). Shares are of that city's catalogued projects. A crowded category raises the bar for a distinct demo moment; it doesn't rule the category out.

| City (n) | Game | Prediction/bet | Payments | DeFi | AI agents | AI app | Social/identity | NFT/ticket/creator | Infra/DePIN | Unknown |
|---|---|---|---|---|---|---|---|---|---|---|
| Pune (5) | 1 | 0 | 1 | 0 | 1 | 0 | 0 | 1 | 1 | 0 |
| Mumbai (9) | 1 | 2 | 0 | 1 | 3 | 0 | 0 | 0 | 2 | 0 |
| New Delhi (5) | 1 | 1 | 0 | 0 | 1 | 0 | 1 | 0 | 0 | 1 |
| Hyderabad (16) | 2 | 1 | 3 | 1 | 2 | 0 | 2 | 2 | 0 | 3 |
| Nagpur (3) | 1 | 0 | 0 | 1 | 0 | 0 | 1 | 0 | 0 | 0 |
| Lucknow (5) | 2 | 0 | 0 | 0 | 0 | 0 | 0 | 1 | 0 | 2 |
| Bangalore (43) | 8 | 6 | 4 | 4 | 8 | 4 | 2 | 3 | 2 | 2 |
| **India total (86)** | **16 (19%)** | **10 (12%)** | **8 (9%)** | **7 (8%)** | **15 (17%)** | **4 (5%)** | **6 (7%)** | **7 (8%)** | **5 (6%)** | **8 (9%)** |
| India, open-theme events only (67) | 16 (24%) | 10 (15%) | 6 (9%) | 7 (10%) | 3 (4%) | 1 (1%) | 6 (9%) | 7 (10%) | 4 (6%) | 7 (10%) |
| India, "Agent Economy" events (19) | 0 | 0 | 2 | 0 | 12 (63%) | 3 | 0 | 0 | 1 | 1 |
| Seoul (23) | 5 | 2 | 1 | 3 | 0 | 0 | 5 | 3 | 1 | 3 |
| Shenzhen (14) | 3 | 1 | 1 | 4 | 1 | 1 | 1 | 0 | 1 | 1 |
| Lagos (9) | 2 | 0 | 4 | 2 | 0 | 1 | 0 | 0 | 0 | 0 |
| Denver/SF/London/Paris (7) | 1 | 1 | 0 | 0 | 1 | 1 | 1 | 0 | 1 | 1 |
| **All (139)** | **27 (19%)** | **14 (10%)** | **14 (10%)** | **16 (12%)** | **17 (12%)** | **7 (5%)** | **13 (9%)** | **10 (7%)** | **8 (6%)** | **13 (9%)** |

**Repeated concepts (observed, opened (git)):**
- Crypto payment links / QR "UPI for crypto": MonadPay+ and MonadPay (Hyderabad), MonadPay (Shenzhen), PayLink (Lagos). Plus voice/chat/SMS payments: EchoPay, MonoSMS, Sendr, PayPilot.
- Cricket/sports betting and group-chat sidebets: Monbetto, CricketX, YOUBET (Telegram), Sidebets (Telegram), Polybetty (Telegram), Molybets.
- Fast up/down price-prediction games: BobaSoda, Hylo, OXGame, BattleMonads.
- Swipe UIs: Monaswipe (tokens), BobaSoda (predictions).
- Collaborative pixel canvas: Canvas (Bangalore), Monad Lisa (Seoul), State Clash (Mumbai).
- AI murder-mystery games: MysteryAI (Bangalore), DeadDrop (Nagpur).
- Stake-to-play arcade / earn-MON arcade: Monad Arcade (Pune), Drippy Cat, x402 gaming paywall, Monarcade.
- NFT ticketing: MonoKen (Pune V1), T-MON (Seoul).
- NFT business/identity cards for networking: MintMe (Seoul), Proof Go (Delhi).
- Farcaster mini apps: Monad Arcade, StakeHub, MonSignal, SignalCast.
- Rotating savings / chit fund: AjoChain (Lagos).
- The Monad Foundation's own "What to Build" list for Blitz (search excerpt · medium): "Social Signals for Degens, AI Agent Payment Rails, Monad Migration MCP, IndieFi, NFT Terminal, Sidebets, and MonadPay". Most of these have already been built at Blitz events: NFT Terminal (Hyderabad #4), Sidebets (Bangalore #24), MonadPay (Hyderabad #12, Shenzhen), MCP4Monad (Shenzhen), SignalCast and MonSignal (social signals). Treat the official list as taken.

**Categories with few entries:** explicit in-room audience participation (4 of 139); AI consumer apps outside agent themes (1 of 67 India open-theme); infra/DePIN/civic (8 of 139); native mobile (2 of 139); location/IRL games (Monad Go, Proof Go's proximity map: 2); creator tools beyond NFT minting (MACHUPS only); music (MusicWithNow only); ticketing (2).

**Most saturated (India open-theme events):** games 24%, prediction/betting 15%, then NFT/creator, DeFi and payments ~10% each.

**Overlap with `defaults.md`:** D1 casual game, D3 cricket prediction, D4 NFT ticketing, D6 bill splitting, D7 ROSCA, D9 raffle, D11 real-time multiplayer and D14 agent wallet all have Blitz precedents above, D4 and D6 at Pune itself.

---

## 5. Platform and submission

- **Submission flow (opened (git), repo READMEs on `main`):** fork `monad-developers/monad-blitz-<city>`, name it after the project with a one-line description, build in the fork. Older repos (Bangalore 2025) then asked for a PR; from 2026 the READMEs say "For next steps head to [Blitz Portal](https://blitz.devnads.com)" (Lagos, London, Berlin). The London transcription says the portal handles "tokens, submission, voting" and that "Submission order sets pitching order". The Pune repo's README (Dec 2025) stops at step 3. A Pune V3 repo hasn't been checked (it may appear on the day; `monad-blitz-pune-v3` and similar names didn't exist on 2026-10-09).
- **Visibility:** repos are public, so peers can inspect code and commit times (R6). Recent PR galleries are sparse: Pune V2 has 2 projects in PRs; NYC, Berlin and Bhopal have 0. Treat the PR set as a sample.
- **Scale:** the Monad blog "Home for Builders" (search excerpt · low-medium) cites ~1,700 unique developers, and "688 attendees and 282 projects deployed across nine events in 2026", about 30 projects per event. The brief expects 50–100 people at Pune V3.
- **No sponsor tracks** at Pune V3 (R14). Some editions add bounties in the info pack (London transcription); none were relayed for Pune.
- **AI coding use is visible and normal:** PR authors include "Claude" (SF #2, Paris #2); several repos ship `AGENTS.md`/`CLAUDE.md` and Monad's `monskills` agent skills (Mumbai V3, Bangalore V4, Lagos). The organizers themselves promote `skills.devnads.com` "coding-assistant skills to build Monad apps" (London transcription). No rule against AI assistance was found.

---

## 6. Sponsor patterns: what Monad Foundation wants, and infra

**Stated wants (Monad Foundation):**
- The Blitz brief, as transcribed for London (opened (git) · medium-high): "Focus on 💡 Novel mechanics & unique ideas · 🧪 Exploring the limits of Monad · ✨ Solving problems in new ways". Its list of "What 600 ms finality unlocks": "Real-time, fully on-chain apps", "AI agents with on-chain state", "High-frequency gaming and DeFi", "Payment rails that feel like Web2", "Computationally heavy ZK and privacy".
- "What to Build" Notion page (search excerpt · medium): the idea list in §4, plus the strongest projects "use Monad's speed, throughput, and low fees to enable new user experiences or backend functions".
- Recent Indian Blitz themes were "The Agent Economy" (Pune V2 Jul 2026, Mumbai V3 Jun 2026, Bangalore V4 Jun 2026; Luma titles via search, plus PR READMEs, opened). Pune V3 returns to "any consumer app" (R1).
- Ecosystem programs after the event: Monad Momentum (testnet product, retention, revenue; search excerpt), MOST, AI Blueprint, Delta V, BuildAnything.so (London transcription).

**Infra seen in builds and its 2026 status:**

| Item | On Monad testnet in 2026? | Free? | Evidence · confidence |
|---|---|---|---|
| Privy embedded wallets (email/social login) | Yes | Monad docs say Privy is among providers "subsidizing usage on Monad Testnet" (search excerpt); a free dev tier is assumed, not verified | Proof Go (Delhi, Mar 2026) and PayLink (Lagos, Apr 2026) use `@privy-io/react-auth` (opened); 6 of 58 2026 PRs with a package.json include Privy · medium-high |
| Para, Mera (account abstraction) | Listed by the organizers | "marked *free*" in the brief | London transcription · medium |
| Dynamic | Yes (used) | unknown | Darwin (Denver, Feb 2026) · medium |
| Scaffold-ETH 2 burner wallet | Yes (any EVM chain) | Free, no signup | State Clash used it for popup-free audience txs (opened) · high |
| RainbowKit / Reown AppKit / wagmi + viem | Yes | Free (Reown needs a free project ID: assumption) | 2026 PRs: viem 41, wagmi 25, RainbowKit 8, Reown 6 of 58 (dependency tally, opened); "viem 2.40+ — native Monad testnet and mainnet support" (London transcription) · high |
| Pyth price feeds | Yes (used Feb 2026) | Pull-oracle update fee (amount unverified) | Hylo (Hyderabad V2) · medium |
| Pyth Entropy (randomness) | **Testnet unconfirmed.** A Monad *mainnet* Entropy address is posted on Pyth's dev forum; Monad is not in the 15 Aug 2026 Entropy deprecation list (ZetaChain, Unichain, Taiko, Tabi, Story, Blast, Etherlink, Sei EVM) | Per-request fee; a forum thread mentions a 40 MON mainnet fee (unverified) | search excerpts · low-medium. Check `docs.pyth.network/entropy/chainlist` on the day, or use commit-reveal / blockhash for non-money demos |
| Gelato VRF | Used on Monad testnet in Aug 2025 | unknown | SponsoredRaffle (Seoul #28, opened) · low for 2026 |
| Switchboard | No evidence on Monad | – | search found Solana only · low |
| Chainlink (data feeds, Data Streams, CCIP) | Used in 2025 Seoul builds; listed by organizers in 2026 | Feeds free to read | Seoul #29, #32, #36, #37 (opened); London transcription · medium |
| Envio HyperIndex | Docs page `docs.envio.dev/docs/HyperIndex/monad-testnet` exists | Free tier unverified | search result title; MonSignal (Bangalore #28) used it · medium |
| Goldsky, Allium, Moralis indexers | Listed by organizers | unknown | London transcription · medium |
| Multisynq (real-time sync without a server) | No Monad integration found; 0 of 139 Blitz projects used it | Free API key from multisynq.io (search excerpt) | low |
| x402 facilitator, MPP (`@monad-crypto/mpp`), ERC-8004 | Yes | "free facilitator hosted by Monad" | London transcription · medium; used in Cachemarket, Cult Simulator plans (opened) |
| `skills.devnads.com` agent skills | Yes | Free | "built-in testnet faucet, contract dev & deploy, frontend dev & deploy, indexer deployment" (London transcription) · medium; `monskills` folders in several 2026 PRs (opened) · high |
| Off-chain real-time state | – | Free tiers | 2026 PRs: Supabase 8, Neon 8, socket.io 1 (dependency tally, opened) · high |

Builds using embedded or burner wallets were a small minority: 7 of 139 flagged EW (`past-submissions.md`).

---

## 7. Monad technical facts (for a 6-hour build)

| Fact | Value | Source · confidence |
|---|---|---|
| Testnet chain ID | **10143** | Official testnets page (search excerpt); used in ≥15 2026 PR configs (opened) · high |
| Testnet public RPC | `https://testnet-rpc.monad.xyz` | The most common RPC in PR configs from May 2025 through Apr 2026 (opened); third-party guides · high. Alternatives seen in PRs: `https://monad-testnet.drpc.org`, `https://rpc-testnet.monadinfra.com`, Alchemy `monad-testnet.g.alchemy.com/v2/<key>` · medium |
| Public RPC rate limits | "Monad public RPC access is rate-limited" | main.net reference (search excerpt) · medium. Limits not quantified (unverified) |
| Testnet explorers | `https://testnet.monadexplorer.com`, `https://testnet.monadvision.com` (Paris Sep 2026 verified a contract there), `https://monad-testnet.socialscan.io`, `testnet.monadscan.com` | PR links (opened) · high that they were in use; which is canonical is unverified |
| Faucet | `https://faucet.monad.xyz` (app hub `https://testnet.monad.xyz`) | Official testnet reference (search excerpt); used in PRs Nov 2025–Sep 2026 (opened) · high. Per-wallet amount and cooldown **unverified** |
| Block time / finality | Mainnet: ~0.3 s blocks, 0.6 s finality (MonadBFT: speculative after 1 slot, full after 2). Docs' current-facts page reportedly says mean block time was ~302 ms in Sep 2026 and to prefer it over older numbers | docs.monad.xyz/ai/current-facts (search excerpt); London brief "Block time 0.3 s / Finality 0.6 s" · medium-high. Older figures (400 ms / 800 ms) appear in the Bangalore V4 playbook. **Testnet block time not confirmed.** |
| Gas charging | Charged on **gas limit, not gas used**; sender is debited `value + gas_bid * gas_limit`; no refunds; wallets should use a small, Monad-specific gas-limit margin | docs.monad.xyz differences, Monad Foundry and wallet-developer pages (search excerpts, consistent across 3 pages) · high |
| Contract size | Max code 128 KB (init code 256 KB) vs 24 KB on Ethereum | docs differences page (search excerpt) · high |
| Tx types | Blob txs (type 3) unsupported; P256 precompile (EIP-7951) supported. One third-party page says EIP-1559 type 2 is unsupported, which contradicts the docs summary | search excerpts · type 3: high; type 2 claim: low, ignore unless deploys fail |
| Mainnet | Live since **24 Nov 2025**, chain ID **143**, MON 18 decimals, RPC `https://rpc.monad.xyz` | Decrypt and GoldRush changelog (search excerpts); Memoria (Bangalore V4) deployed to chain 143 (opened) · high |
| Tooling | Standard Solidity/Foundry/Hardhat/viem/ethers; viem ≥2.40 has Monad chains; Monad Foundry fork; `docs.monad.xyz/llms-full.txt` for coding agents | London transcription · medium-high |
| Throughput headline | "500M gas / sec", "10,000" TPS sustained | London transcription of organizer slides · medium (use as pitch numbers only) |

---

## 8. Judge context

Named hosts (user-relayed from the Pune V3 Luma page): **Arsh Goyal, Kushal Vijay, Kartikey Garg, Jigar Vyas**, plus Monad Foundation and The AI Collective. Two searches found **no public role or evaluation statements connecting these names to Monad**; I stopped there under the tight budget. A Monad Foundation "Ecosystem India Lead" job listing exists (search excerpt), which doesn't identify anyone. Voting is by peers and possibly a judge half (R7). Don't tailor ideas to individuals.

---

## 9. Confidence notes

- **High:** Pune V3 theme, peer judging, prizes and team size (user-relayed from Luma); testnet chain ID, RPC, faucet, gas-limit charging, contract size, mainnet live; the composition of past PR submissions (opened).
- **Medium-high:** 3-minute demos; deploy gates (live web URL, contracts deployed that day, public repo, README, fork); voting criteria; voting during demos plus 15 minutes; portal-based submission. Each rests on the London participant transcription plus at least one independent source (Bangalore V4 playbook, Paris README, Notion search excerpts).
- **Medium:** 50/50 judge/peer weighting (two participant transcriptions; conflicts with "No official judges" and with Pune V3's "Peer-Judged"); pitching order equals submission order (London only); infra availability rows marked medium.
- **Low / unknown:** any winner identity or winner base rate (none found); ballot format (one vote vs top-N vs scores); testnet block time; Pyth Entropy on testnet; faucet limits; Pune V3 Luma URL slug says "sep-2026" while the brief says 10 Oct (possible reschedule, irrelevant to judging).
- **Sampling bias:** 2026 events route submissions through the Blitz Portal, so PR samples for 2026 (especially Pune V2: 2 projects) under-represent the field. Category shares lean on 2025 events and on Bangalore (43 of 139).
- Shares are observed patterns in submissions, not evidence of what wins.

---

## 10. What past winners already built

Unknown: no winners identified (§3). The nearest usable record is what **entrants** already built, especially in Pune and nearby cities:
- **Pune V1:** split-payments mobile app (CrackPay), NFT ticketing (MonoKen), stake-to-play arcade vs an AI bot (Monad Arcade).
- **Pune V2 (agents):** staked uptime oracle (Oracle), AI agent that grants MON (PiggyBag).
- **Mumbai (Feb 2026, ~150 km away; possible audience overlap):** Farcaster prediction markets (StakeHub), Polymarket-feed trading UI, a real-time pixel canvas visualising parallel execution (State Clash), AI sentiment oracle, DePIN noise map, farmer micro-loans.
- Everything on the official "What to Build" list (§4).

A Pune V3 idea that matches any of these needs a clearly different user, job or mechanism to clear the novelty criterion (R9, R12).

---

## 11. Research log

**Web searches (27; 4 extended; budget ~25):**
1. `Monad Blitz "Rules & Guidelines" hackathon submission testnet` (standard): Rules & Guidelines and Ankara info pack excerpts, blog.
2. `Monad Blitz Pune winners` (standard): only chess results.
3. `"Monad Blitz" winner first place project` (standard): prize amounts, no winners.
4. `"Monad Blitz" Mumbai OR Bangalore OR Hyderabad OR Delhi won hackathon linkedin` (standard): listings, no winners.
5. `"Monad Blitz" "1st place" OR "first prize" OR "won" project built` (extended): Luma pages incl. Pune V2 "The Agent Economy", Pune V3 slug.
6. `Monad Blitz Pune luma peer-judged December 2025` (standard): nothing for Pune.
7. `"won" "Monad Blitz" hackathon my team built` (extended): Denver peer-judged; no winners.
8. `"Monad Blitz" winners announced congratulations` (standard): no winners.
9. `"Monad Blitz" Pune winner linkedin "Monad Arcade" OR "MonoKen" OR "CrackPay" OR "PiggyBag"` (standard): nothing.
10. `"Monad Blitz" "State Clash" OR "Hylo" OR "StakeHub" OR "Monaswipe" OR "Eat My Token" winner` (standard): nothing.
11. `Monad Blitz 深圳 获奖项目 第一名` (standard): TechFlow says audience voting; Chinese editions use joint official + developer votes; no winners.
12. `Monad Blitz Pune V3 luma Ideas to Impacts consumer app` (standard): consumer themes elsewhere.
13. `Monad testnet chain ID 10143 RPC testnet-rpc.monad.xyz block time finality docs` (standard).
14. `Monad gas limit charged not gas used contract size limit 128 kb docs.monad.xyz differences from Ethereum` (standard).
15. `Pyth Entropy Monad testnet contract address VRF randomness Switchboard Gelato VRF Monad` (standard).
16. `Monad mainnet launch date November 2025 chain ID 143 MON` (standard).
17. `Pyth Entropy deprecation August 15 2026 selected chains list Monad testnet` (standard).
18. `Privy embedded wallet Monad testnet support OR Multisynq Monad real-time multiplayer` (standard).
19. `Monad Foundation consumer apps "what to build" ideas Monad DevRel 2026 RFP mission` (standard): What to Build list, blog stats.
20. `Monad Blitz demo voting "vote" builders "top 3" form how winners chosen demo minutes` (standard): Judging Process & Criteria excerpt.
21. `monad-foundation.notion.site "Judging Process" Criteria Monad Blitz voting platform criteria` (extended): Pune V2 "Judging & Community Vote".
22. `Monad Blitz info pack schedule "submission deadline" demo "minutes" per team pitch` (standard): timelines.
23. `blitz.devnads.com Blitz Portal Monad projects` (standard): not indexed.
24. `Arsh Goyal Monad OR "Kushal Vijay" OR "Kartikey Garg" Monad OR "Jigar Vyas" Pune hackathon` (standard): nothing relevant.
25. `"Monad Blitz" "2nd place" OR "runner up" OR "secured first" OR "bagged" linkedin.com` (extended): nothing.
26. `"Arsh Goyal" Monad Foundation India OR "Kartikey Garg" Monad OR "Kushal Vijay" Monad devrel` (standard): job listings only.
27. `docs.monad.xyz "current facts" testnet block time 400ms OR 300ms finality faucet testnet.monad.xyz` (standard).

**Git (opened):** `git ls-remote` probes of 40 candidate `monad-developers/monad-blitz-<city>` names. Repos with PRs: pune 7, mumbai 12, bangalore 49, delhi 5, hyderabad 19, nagpur 4, bhopal 2, lucknow 6, seoul 38, shenzhen 31, lagos 13, denver 6, sf 3, nyc 2, london 3, paris 2, berlin 1. No PRs or no repo: pune-v2/-2/-v3/-3, mumbai-2/-v2, delhi-2, bangalore-2, chennai, kolkata, jaipur, ahmedabad, indore, goa, kochi, chandigarh, gurgaon, noida, tokyo, singapore, hongkong, taipei, istanbul, dubai, bogota, buenos-aires, saopaulo. Then `git fetch --depth 1` of `refs/heads/*` and `refs/pull/*/head` for the 17 repos, and reads of READMEs, submission `.md` files, `package.json`, `<title>` tags and contract names. No code from the repos was executed. Clones are in the scratchpad under `blitz-repos/`.

**Refused or not attempted:** per the caller's report, WebFetch/curl are refused for notion.site, docs.monad.xyz, x.com, youtube, luma, devfolio and api.github.com. I did not retry them, use mirrors, or route around them, and did not clone the Monad docs from another host. So every Notion, Luma, docs and blog item above is a search excerpt (not opened). `blitz.devnads.com` was not fetched (same policy; not indexed by search).

**Leads (not opened):** `https://monad-foundation.notion.site/Rules-Guidelines-IMPORTANT-PLEASE-READ-2726367594f281228cecc8b78c03751a`; `https://monad-foundation.notion.site/Judging-Process-Criteria-2736367594f280dfa8d8f8afa9d0a06e`; `https://monad-foundation.notion.site/What-to-Build-2726367594f2819892dbf88253b7031c`; `https://monad-foundation.notion.site/Submission-Process-cc66367594f2837c898701aabd948402`; `https://luma.com/monad-blitz-pune-sep-2026` (Pune V3); `https://luma.com/blitz-pune-july-2026` (Pune V2); `https://monad.xyz/blog/home-for-builders`; `https://docs.monad.xyz/ai/current-facts.md`; `https://docs.monad.xyz/developer-essentials/differences`; `https://docs.monad.xyz/developer-essentials/testnets`; `https://docs.monad.xyz/tooling-and-infra/wallet-infra/embedded-wallets`; `https://dev-forum.pyth.network/t/deprecation-notice-pyth-entropy-deprecation-for-selected-chains-august-15-2026/816`; `https://www.techflowpost.com/en-US/newsletter/85963`.
