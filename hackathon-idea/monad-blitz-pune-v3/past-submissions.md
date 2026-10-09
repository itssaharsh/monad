# Past Monad Blitz submissions (raw list)

**Source and method (how: opened).** Every row comes from a pull request against a public `monad-developers/monad-blitz-<city>` repo, read locally after `git fetch --depth 1 origin '+refs/pull/*/head:refs/remotes/pr/*'` (2026-10-09). For each PR I read the root README (or the first non-boilerplate README / submission `.md`), and where there was none, the page `<title>`, `package.json` name/description and contract file names. PR links follow the pattern `https://github.com/monad-developers/monad-blitz-<city>/pull/<n>`. Dates are the PR head commit date (a proxy for the event date).

**Coverage caveat (important).** PRs are not the full gallery. Repos created from late 2025 onward stop the README at "fork" and, from 2026, point to a separate **Blitz Portal** (`blitz.devnads.com`) for submission and voting (see `monad-blitz-lagos`, `-london`, `-berlin` READMEs on `main`). So recent events have only a few PRs each (Pune V2: 2 projects; NYC, Berlin, Bhopal: 0). A Monad blog post, as summarised by web search (not opened), cites ~282 projects across nine 2026 events (~31 per event), so this list is a sample, not a census, especially for 2026. Organizer PRs (Harpal Jadeja) and empty/test PRs are excluded; duplicate PRs of one project are merged.

**Category codes (one primary per project):** GAME = game (arcade, skill, RPG, multiplayer; wager allowed if gameplay-first) · BET = prediction market / betting / raffle / wagering-first · PAY = payments, payment links, splitting, escrow · DEFI = trading, swaps, AMM, lending, savings, launchpads, insurance · AGENT = AI-agent economy / agent infra · AIAPP = AI-centric consumer app with an on-chain hook · SOCIAL = social, identity, community, governance, credentials · NFT = NFT, ticketing, creator/IP tools · INFRA = devtools, oracle, DePIN/civic · UNK = could not identify / trivial.

**Flags:** RT = real-time multiplayer or shared live state · AUD = explicitly designed for in-room audience participation · AI = uses an LLM/ML model · EW = no-extension wallet (embedded, social login, burner, custodial bot wallet, AA) · FC = Farcaster mini app · BOT = Telegram / WhatsApp / SMS interface · MOB = native mobile · URL = README shows a live URL.

---

## India

### Pune — `monad-blitz-pune` (5 projects)
| Event (date) | PR | Project | What it is | Cat | Flags |
|---|---|---|---|---|---|
| V1 (2025-12-13) | #2 | CrackPay | Group expense splitting and payment requests; Expo mobile app + Foundry contracts (ContactBook, GroupManager, SplitCalculator), Reown | PAY | MOB |
| V1 | #3 | MonoKen | "Decentralized event ticketing platform with NFT tickets on Monad" | NFT | |
| V1 | #4 | Monad Arcade | "Stake crypto. Play games. Win 90% of the pool." Stake vs an AI bot; built from the Monad Farcaster mini-app template | GAME | AI, FC |
| V2 "The Agent Economy" (2026-07-04/05) | #5, #7 | Oracle | Decentralised uptime monitoring: escrow-funded jobs, workers stake MON on reports, 3-minute challenge window | INFRA | |
| V2 | #6 | PiggyBag | "A platform run by an AI agent that funds early-stage products"; agent (gpt-4.1-mini) interviews founders, sends 1–5 MON | AGENT | AI |

### Mumbai — `monad-blitz-mumbai` (9 projects)
| Event | PR | Project | What it is | Cat | Flags |
|---|---|---|---|---|---|
| V2 (2026-02-22) | #1 | StakeHub | "Prediction Markets, Native to Farcaster"; live pool bars, SSE updates | BET | FC, RT |
| V2 | #2 | AgriFi | On-chain micro-lending for farmers (credit NFT + loan contract) | DEFI | |
| V2 | #3 | Mon-o-Poly V2 | Trading UI over Polymarket Gamma API markets with Monad contracts | BET | |
| V2 | #4 | State Clash | "Real-time multiplayer 50x50 collaborative pixel canvas" that visualises parallel execution and same-block state collisions; SE-2 burner wallet so users "paint instantly without signing", WebSocket event stream | GAME | RT, AUD, EW |
| V2 | #5 | SentimentFi | Reddit/news sentiment via HuggingFace model pushed on-chain as an oracle score | INFRA | AI |
| V2 | #6 | MumbaiNoise | DePIN: phones as noise sensors, heatmap for the municipal corporation | INFRA | |
| V3 "The Agent Economy" (2026-06-20) | #8, #9, #10 | Cult Simulator / KIRMADA | AI "prophet" agents form ideologies, schisms recorded on-chain (ERC-8004/x402 plan); "Audience votes" on prophet debates; 3 PRs, likely one or two teams | AGENT | AI |
| V3 | #11 | (agent court backend) | Agents, tasks, disputes, "court" and "mitosis" services | AGENT | AI |
| V3 | #12 | AgentStaker / Monad ArenaX | AI agents forecast and stake in a prediction arena; bet-slip NFTs | AGENT | AI, URL |

### New Delhi — `monad-blitz-delhi` (5 projects)
| Event | PR | Project | What it is | Cat | Flags |
|---|---|---|---|---|---|
| V1 (2025-06-14) | #1 | AgentBazaar | Community-voted hub where AI agents compete for attention | AGENT | AI |
| V1 | #2 | Excalibur | "Multiplayer shooting game built with React-Three, Playroom" (Submission.md; demo video link) | GAME | RT |
| 2026-03-28 | #3 | Molybets | Prediction market + sidebet factory, Farcaster-frame mock, "Added Live Demo Link" | BET | URL |
| 2026-03-28 | #4 | wave_app | Flutter app, no description | UNK | MOB |
| 2026-03-28 | #5 | Proof Go | Mint identity cards, share QR profiles, "meet nearby people, and collect profiles on-chain"; Privy email login, proximity map, leaderboard | SOCIAL | EW, URL |

### Hyderabad — `monad-blitz-hyderabad` (16 projects)
| Event | PR | Project | What it is | Cat | Flags |
|---|---|---|---|---|---|
| V1 (2025-07-12) | #3 | Drippy Cat | Flappy-style arcade game, collect coins, redeem MON | GAME | |
| V1 (2025-08-08) | #4 | NFT Terminal | NFT terminal factory + token-gating verifier | NFT | |
| V1 | #5 | IPVerse | IP assets/companies/investment contracts | NFT | |
| V1 | #6 | (unnamed) | Next.js shell, no description | UNK | |
| V1 | #7 | MonadPay+ | "The UPI for crypto payments": payment links, dynamic QR, loyalty NFTs | PAY | |
| V1 | #8 | MonoSMS | "SMS-based on-chain execution via Monad + Brewit" (Twilio + LLM parser); commit says "moved code from original repo" | PAY | AI, BOT |
| V1 | #9 | Number storage | Beginner set/get number dApp on Sepolia (not Monad) | UNK | URL |
| V1 | #10 | MonadScope | Wallet portfolio analyzer with token risk scores | DEFI | |
| V1 | #11 | CertificateNFT | Certificate NFT mint scripts | SOCIAL | |
| V1 | #12 | MonadPay | "Payments that fit in a URL": deeplink payment requests | PAY | |
| V2 (2026-02-28) | #13 | ShipOrShame | "Commit. Stake. Ship. Or Lose." productivity stake | SOCIAL | |
| V2 | #14, #15 | Hylo / TradeBlitz | High-frequency "Box Chart" price-prediction arena; Pyth feeds; off-chain game loop, on-chain treasury + ECDSA settlement | BET | RT |
| V2 | #16 | Molfi | "Let ClawBots Run the Market": AI trading agents with vaults | AGENT | AI |
| V2 | #17 | the-purple-pulse | Screenshots + PDF only | UNK | |
| V2 | #18 | X402 Gasless Gaming Paywall | Arcade game behind x402 payments (README says Cronos testnet) | GAME | |
| V2 | #19 | AgentHub | Marketplace of AI trading agents that snipe/launch memecoins on nad.fun | AGENT | AI |

### Nagpur — `monad-blitz-nagpur` (3 projects, 2026-02-08)
| PR | Project | What it is | Cat | Flags |
|---|---|---|---|---|
| #2 | Orbit | MON/AUSD single-sided Uniswap v4 LP keeper | DEFI | |
| #3 | BlitzBoard | Voting platform for hackathons/events: event codes, QR join, real-time leaderboards, AI-agent consensus votes | SOCIAL | RT, AUD, AI |
| #4 | DeadDrop | "On-Chain Mystery Game": AI/RAG murder mystery with NFTs | GAME | AI |

### Bhopal — `monad-blitz-bhopal` (0 projects; organizer PRs only, 2025-12)

### Lucknow — `monad-blitz-lucknow` (5 projects, 2025-08-02; READMEs not edited)
| PR | Project | What it is | Cat | Flags |
|---|---|---|---|---|
| #2 | ghoulrun | Game with checkpoint claims | GAME | |
| #3 | InfluxXSpace | Folder only | UNK | |
| #4 | (Mint) | NFT mint/creator gallery with Pinata | NFT | |
| #5 | Reflect – Web3 Games | Game lobby + games | GAME | |
| #6 | new_hardhat | Hardhat folder only | UNK | |

### Bangalore — `monad-blitz-bangalore` (43 projects)
| Event | PR | Project | What it is | Cat | Flags |
|---|---|---|---|---|---|
| V1 (2025-05-31) | #2, #14 | Monaswipe | "Tinder-style swipe" token discovery; swipe right to buy | DEFI | |
| V1 | #3 | CleanChain | Neighbourhood waste-collection logging | INFRA | |
| V1 | #4 | EchoPay | Voice/NLP crypto payments | PAY | AI |
| V1 | #5 | Monbetto | "Degen Sports Betting for Monad" (cricket) | BET | |
| V1 | #6 | Eat My Token | "Simple agar.io type game for eating your token" | GAME | RT, URL |
| V1 | #7 | CricketX | "Degen betting app for live cricket games" | BET | |
| V1 | #10 | Monad NFT Marketplace | Price-reactive NFTs, rentals | NFT | |
| V1 | #11 | (v0 project) | No description | UNK | |
| V1 | #12 | Marketplace | IPFS-backed marketplace | NFT | |
| V1 | #13 | YOUBET | "Bet on anything"; website + Telegram bot | BET | BOT, URL |
| V1 | #16 | type-race | Typing race with lobby/rooms | GAME | RT |
| V1 | #17 | ModAI | AI chat that can check balance / send tokens | AIAPP | AI |
| V1 | #18 | GriffinLock | Escrow (Lovable-generated UI) | PAY | |
| V1 | #19, #20 | LaunchPass (VibeCity) | ERC-1155 launch passes, discount/creator tokens | NFT | |
| 2025-06-19 | #8 | Monad FaaS | Serverless functions triggered on-chain | INFRA | |
| V2 (2025-07-15 / 08-02) | #15 | ChainJump | Platformer game + level editor, earn rewards | GAME | |
| V2 | #23 | Canvas | Collaborative pixel canvas, own pixels | GAME | RT, URL |
| V2 | #24 | Sidebets | "Turn trash talk into real stakes": `/sidebet` in group chat | BET | BOT, EW |
| V2 | #25 | BitMon | Bitcoin–Monad atomic swaps (HTLC) | DEFI | |
| V2 | #26 | PokeMon | Phaser + Colyseus multiplayer world with WebRTC | GAME | RT |
| V2 | #27 | Tapnad | Tap race game (commit: "Imported codebase from old repo") | GAME | RT |
| V3 (2025-11-29) | #28 | MonSignal | Farcaster mini app + Envio indexer for trading signals | SOCIAL | FC |
| V3 | #30 | ZYURA | Instant flight-delay insurance | DEFI | |
| V3 | #31 | BobaSoda | "1 minute fast paced prediction game", swipe up/down | BET | |
| V3 | #32 | VibeFi | "The Vibe Check Prediction Market" | BET | |
| V3 | #33 | SAMM | Sharded AMM (research paper) | DEFI | URL |
| V3 | #34 | MysteryAI | AI murder-mystery game (Gemini), rewards to wallets | GAME | AI |
| 2026-03-11 | #9 | GhostPass | Anonymous wallet-ownership verification | SOCIAL | |
| V4 "The Agent Economy" (2026-06-07) | #35, #38 | krow | AI-verified freelance escrow (6-agent "orchestra" checks GitHub PRs) | AGENT | AI |
| V4 | #36 | Parallel AI Agent Orchestration | Concurrent agent execution on Monad | AGENT | AI |
| V4 | #37 | GuardRail Pay | Payment firewall for AI agents | AGENT | AI |
| V4 | #39 | StealthMode | Stealth-address (ERC-5564) private payments | PAY | |
| V4 | #40 | Shared Agent Notebook | Tamper-evident scratchpad for multi-agent workflows | AGENT | AI |
| V4 | #41 | Memoria | Provenance/royalties for AI memory; deployed on Monad mainnet (chain 143) | AGENT | AI |
| V4 | #42 | AgentMandi | Manager agent hires and pays specialist agents | AGENT | AI |
| V4 | #43 | Vox Protocol | Identity/proof-of-call for voice AI agents | AGENT | AI |
| V4 | #44 | ProofOfSynergy | AI voice interview → skill attestations | AIAPP | AI |
| V4 | #45 | (escrow inspection) | Property inspection escrow registry | PAY | |
| V4 | #46 | Notion_Hackthon.zip | Zip only | UNK | |
| V4 | #47 | Agent Subconscious | Agent memory on-chain | AGENT | AI |
| V4 | #48 | Contract Copilot AI | PDF contract risk analysis (Gemini) | AIAPP | AI |
| V4 | #49 | VoiceForms AI | Voice-conversation forms with on-chain credentials | AIAPP | AI |
| 2026-08-11 (V5?) | #29 | Monad Go | Location-based game: hunt and mine MON faucets on a live map | GAME | |

(Bangalore #21 and #22 are Shenzhen submission files misfiled into this repo; excluded.)

---

## Outside India

### Seoul — `monad-blitz-seoul` (23 projects)
| Event | PR | Project | What it is | Cat | Flags |
|---|---|---|---|---|---|
| 1st (2025-07-12) | #2, #3, #14 | (delivery protocol) | Delivery protocol with delivery fee events (SE-2) | PAY | |
| 1st | #4, #7, #8 | InstantSwap | Swap contract | DEFI | |
| 1st | #5, #9 | MonadApp | Elixir/Phoenix app, no description | UNK | |
| 1st | #6 | Omni Oracle | Oracle that can input/retrieve any data | INFRA | |
| 1st | #10 | Monad Lisa | Pixel canvas with live-users panel and tx history | GAME | RT |
| 1st | #11 | MonaMiner | On-chain idle mining game with NFT miners, gacha | GAME | |
| 1st | #12 | TrendLink | Web3 search widget with rewards; Web3Auth social login | SOCIAL | EW |
| 1st | #13 | MintMe | NFT business cards for networking | SOCIAL | |
| 1st | #15 | MonaTx RPG | Transaction-driven RPG gamification layer | GAME | |
| 1st | #17 | Team Undertaker | Results file only | UNK | |
| 1st | #18 | USDx | Token minter / stable token | DEFI | |
| 2nd (2025-08-30) | #19, #20, #29 | BattleMonads | Monster battles where Chainlink price moves decide fights; bets on monsters | GAME | |
| 2nd | #21 | (Flask API) | Backend scaffold | UNK | |
| 2nd | #22–#24 | MonaDAO | DAO + token | SOCIAL | |
| 2nd | #25 | Monaddit | Reddit-style community, stake to post | SOCIAL | |
| 2nd | #27 | MusicWithNow | Shared music room with chat and screen share | SOCIAL | RT |
| 2nd | #28 | SponsoredRaffle | Raffle using Gelato VRF | BET | |
| 3rd (2025-11-15) | #30 | Volatility Cats | NFTs imprinted with market state at mint | NFT | |
| 3rd | #31, #35 | T-MON | First-come ticketing using high TPS | NFT | |
| 3rd | #32, #34 | CCIP Airdrop | USD→USDC CCIP airdrop | DEFI | |
| 3rd | #33 | Claw machine / Colosseum | Character NFTs battle in a colosseum (v0 UI) | GAME | |
| 3rd | #36 | Minecraft PFP | PFP NFTs with Chainlink feeds + CCIP | NFT | |
| 3rd | #37 | OXGame | O/X price rounds every 5 s via Chainlink Data Streams | BET | RT |

### Shenzhen — `monad-blitz-shenzhen` (14 projects, 2025-06-08; first ever Blitz; submissions are `.md` files)
| Project | What it is | Cat | Flags |
|---|---|---|---|
| Torch | Gamified token launch: rounds, burn-to-win leaderboard | DEFI | |
| GAME OF LIFE | Conway's Life with NFT-unlocked pro features | GAME | |
| AI Scorer | Essay bounties scored by AI | AIAPP | AI |
| MCP4Monad | MCP server for Monad testnet data | INFRA | AI |
| Immortal (不朽) | Quiz arena with AI referee; "audience can cheer contestants in real time and replace contestants who keep answering wrong" | GAME | RT, AUD, AI, URL |
| SignalCast | Farcaster mini app; AI pet tracks friends' trades | SOCIAL | FC, AI |
| life++ SatoshiFlow | AI agent economic sovereignty | AGENT | AI |
| YD-Monade-Happy-Pro (ZangAi) | End-of-life care service platform | UNK | |
| MonadPay | Deeplink payment URLs | PAY | URL |
| Polybetty | Telegram betting bot | BET | BOT |
| Shortzy | Short meme coins | DEFI | |
| MonadArena | Trading-competition rooms | DEFI | URL |
| SimpleMarket | Futures / pre-market | DEFI | URL |
| 链游五子棋 (Gomoku) | On-chain gomoku | GAME | RT, URL |

### Lagos — `monad-blitz-lagos` (9 projects, 2026-04-11)
| PR | Project | What it is | Cat | Flags |
|---|---|---|---|---|
| #3 | Flow-State | Per-second streaming ride payments | PAY | |
| #4, #13 | PayLink | Payment links; Privy | PAY | EW |
| #2, #5, #6 | Monarcade | Brands fund game challenges with on-chain prize pools | GAME | |
| #7 | Sendr | WhatsApp-style AI payments | PAY | AI, BOT |
| #8 | MONA | AI shopping chat with cart | AIAPP | AI |
| #9 | AjoChain | Rotating savings (ajo) with AI matching | DEFI | AI |
| #10 | FairDrops | Giveaways decided by skill games (PR updated 2026-10-05 as a refactor) | GAME | |
| #11 | PayPilot | Natural-language payment rules | PAY | AI |
| #12 | Monad-Wealth | Gasless AI savings (ERC-4337 paymaster) | DEFI | AI, EW |

### Denver (2026-02-17), SF (2025-12-06), London (2026-08-08), Paris (2026-09-23) — 7 projects
| Repo / PR | Project | What it is | Cat | Flags |
|---|---|---|---|---|
| denver #2 | (SE-2) | Mascot page, no description | UNK | |
| denver #3, #4 | Cachemarket | First requester pays to seed an on-chain data cache, later readers pay ~100x less; x402 | INFRA | |
| denver #5, #6 | Darwin | Marketplace for AI agents and humans; Dynamic wallets | AGENT | AI, EW |
| sf #2, #3 | MACHUPS | "AI Brand Generator: From Idea to Brand in 3 Minutes" | AIAPP | AI |
| london #2 | PodShop Arena | Broker-signed track records anchored on Monad | SOCIAL | |
| london #3 | BoostBounty | Gaming bounties, friend bets, rock-paper-scissors escrow | BET | |
| paris #2 | BRIC À BRAC | A phone films the room, on-device AI picks an object, a live Dutch auction runs; "Le public regarde le live sur son téléphone" (buyers join by QR, shared emoji reactions) | GAME | RT, AUD, AI |

NYC, Berlin: organizer PRs only.

---

## Totals used in `judging-intel.md`

139 categorised projects (India 86, outside India 53).

| Category | All 139 | India 86 | India open-theme events (67) | India "Agent Economy" events (19: Pune V2, Mumbai V3, Bangalore V4) |
|---|---|---|---|---|
| GAME | 27 (19%) | 16 | 16 (24%) | 0 |
| BET | 14 (10%) | 10 | 10 (15%) | 0 |
| PAY | 14 (10%) | 8 | 6 (9%) | 2 |
| DEFI | 16 (12%) | 7 | 7 (10%) | 0 |
| AGENT | 17 (12%) | 15 | 3 (4%) | 12 (63%) |
| AIAPP | 7 (5%) | 4 | 1 (1%) | 3 |
| SOCIAL | 13 (9%) | 6 | 6 (9%) | 0 |
| NFT | 10 (7%) | 7 | 7 (10%) | 0 |
| INFRA | 8 (6%) | 5 | 4 (6%) | 1 |
| UNK | 13 (9%) | 8 | 7 (10%) | 1 |

Flag counts across all 139 (counted from the tables above): AI 39, RT 16, URL 13, EW 7, BOT 5, FC 4, AUD 4 (State Clash, BlitzBoard, Immortal, BRIC À BRAC), MOB 2.
