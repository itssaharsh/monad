# Idea package: monad-blitz-pune-v3

Mode hackathon · context general · depth quick · discovery blind_subagent · 12 problems across 4 user groups · 2026-10-10

Request: make the idea / brainstorm an idea for Monad Blitz Pune V3; the most priority is to win

Ideas are listed in the order they were found. The order is not a ranking.

## Event

Monad Blitz Pune V3 · Luma + GitHub fork (or Blitz Portal) · window 1 day (about 7 build hours) · live 3-minute demos; phone voting by participants during and 15 min after demos; possibly 50% judges · https://luma.com/MonadFoundation

| Criterion | Weight | Quote | Source | Implication |
|---|---|---|---|---|
| Novelty & originality | equal | Novelty & originality | assumed | avoid the saturated categories (games 24%, betting 15% of 67 Indian Blitz entries) |
| Innovative mechanics on Monad | equal | Innovative mechanics, especially ones that use Monad's potential (paraphrase of the London transcription) | assumed | the core mechanic must need sub-second blocks, visibly |
| Problem-solving | equal | does it creatively address a real or interesting challenge for consumer applications? | assumed | anchor on a real, local, evidenced problem |
| Learning & experimentation | equal | Learning & experimentation | assumed | show the mechanism and its honest limits to a developer room |

## Parchi

Problem: hypothesis (weak) · Wedge: untested

**Concept:** Parchi runs a society's parking draw that nobody can rig: everyone in the room adds randomness from their phone, the result lands in about a second, and anyone can check it.

**Problem:** Parking and amenity draws in Indian housing societies get called rigged: members left off the list, slots reassigned, draws re-run; disputes go to the registrar or cooperative court (E13, E14, E15).

**User:** a resident at the AGM allotment; and the committee member who wants a draw nobody can accuse them of rigging

**Why it matters:** a contested draw costs months of disputes; the suspicion comes from the drawer being able to re-run or edit the list in private

**Core workflow:** committee creates a draw with slots and the eligible flats -> residents scan the QR, pick their flat, tap to add a secret (commit) -> committee closes entries: the list is frozen on-chain -> phones reveal secrets automatically -> after the reveal window, the seed mixes all reveals with a block hash from after the close -> the contract allocates slots with a deterministic shuffle -> every phone shows its result and a Verify button that recomputes it from chain data; a second draw attempt reverts

**Ai role:** n/a: no model in the loop

**Non ai product:** the whole product: draw registry, frozen eligible lists, commit-reveal contribution, on-chain allocation, gasless relayer, verifier page and each society's draw history

**Data loop:** every draw leaves a public, re-checkable record per society; later draws (re-allotment, guest parking rotation) build on it

**Hard part:** a fair seed with ~100 contributors in under a minute: commit-reveal with a post-close block hash so a withholding last revealer gains nothing, gasless signed entries relayed without nonce collisions, gas limits sized because Monad charges the limit, and an in-browser verifier that reproduces the contract's shuffle bit for bit; the non-reproducible part is the public per-society record of draws, not the code

**Technical implementation:** Solidity contract on Monad testnet (draw state machine, EIP-712 signed joins and reveals, partial Fisher-Yates); Next.js on Vercel with a relayer route holding a small pool of relayer keys; phones hold a burner key in localStorage; one cached read endpoint so 100 phones don't hit the public RPC; viem everywhere; no wallet extension

**Mvp scope:** IN: create draw, QR join with flat label, commit, close, auto-reveal, draw, allocation board, phone result, verifier, re-run revert, simulated-residents button for rehearsal. OUT: identity checks for flats, priority rules (EV, senior), proxies for absent members, mainnet

**Differentiation:** versus paper chits: a frozen list and a result anyone recomputes; versus RANDOM.ORG: the committee can't choose when or whether to re-run; versus past Blitz raffles: allocation of named slots with randomness from the room, not an oracle

**Risks:** venue Wi-Fi or RPC rate limits during the live round (mitigated: relayer + cached reads + simulated residents fallback); the room reads it as 'just a raffle' (mitigated: lead with the society problem and the frozen list); entitlement disputes remain unsolved (disclosed as a scope limit)

**Buildable in event:** solo with a coding agent in ~6 h: contract + tests 1.5 h, relayer + reads 1 h, five screens 2.5 h, deploy and rehearsal 1 h; residents can be simulated (labelled) if the room doesn't join

**Wedge:** the eligible list is frozen on-chain before any randomness exists, every resident in the room adds randomness from their phone with no wallet or gas, and any phone can recompute the result; paper chits and third-party draws can't show any of the three

Fingerprint: domain: community governance; user: a housing-society resident at the AGM where parking slots are drawn; job: accept the allocation of scarce parking or amenity slots; mechanism: eligible list frozen on-chain before randomness exists, randomness contributed by every resident in the room, deterministic on-chain allocation, a verifier that recomputes it on any phone; mechanism_family: verify; buyer_or_demo: the whole room joins a draw by QR, adds randomness, the allocation lands in about a second, every phone verifies it, and the host's re-run attempt reverts

### Wrapper filter (Stage 4b/6W)

**What the product is:** a draw registry for housing societies that freezes the eligible list, collects randomness from residents' phones, allocates slots on-chain and lets anyone recompute the result

**Model in the core loop:** False

**Reduces to:**  — fair summary: 

**AI leverage:**  — why plain code isn't enough: 

**Without the model:**  (survives: )

**Wrapper shape declared:**  — differs: 

| Wrapper-smell question | Answer |
|---|---|
| q1_one_api_call |  |
| q2_textbox_ui |  |
| q3_prompt_differentiator |  |
| q4_workflow_outside_model |  |
| q5_touches_real_systems |  |
| q6_improves_with_use |  |
| q7_pays_after_novelty |  |
| q8_chatgpt_would_suggest |  |

### Authenticity signals

| Signal | Group | How it holds in this product | Basis |
|---|---|---|---|
| narrow_persona | workflow | a resident at the AGM where stilt-parking slots are drawn among more members than slots | evidence |
| existing_workaround | workflow | chits in a bowl at the AGM, then complaints to the deputy registrar | evidence |
| domain_logic | workflow | eligible flats fixed before entries open; one entry per flat; slots named P1..Pn | commitment |
| accumulated_history | compounding | each society's past draws stay public and re-checkable, so a re-allotment cites the earlier record | commitment |
| deterministic_core | substance | the shuffle is a pure function of the seed and the frozen list; the verifier test reproduces the contract's output | commitment |
| distribution_or_trust | substance | neither the committee nor the app operator can re-run or edit the list, which is the trust the evidence says is missing | evidence |

**Moat:** accumulated history; regulatory or trust position — no model involved; the value is a public record of each society's draws that its residents already trust, which a competitor can't backfill

### What they do today

| Alternative | What they actually do | What changes with this |
|---|---|---|
| paper | chits in a steel bowl drawn at the AGM | the list is fixed in public first and anyone can recompute the result afterwards |
| existing saas | society apps record the outcome typed in by the committee | the outcome is produced, not typed in |

**Why now:** cost · 2025-11 · a public EVM chain with sub-second blocks and near-zero fees reached mainnet (Monad, Nov 2025; testnet free) · threshold: about 100 signed contributions plus the draw inside about 60 seconds at roughly zero cost per resident (assumption) · couldn't before: a 60-flat society AGM with no crypto-holding residents and no budget for a paid third party

### Evidence

Overall weak: three independent organisations (news, a housing-law column, advocates) describe contested society draws, but every page was blocked from this container, so they are leads read as search summaries; the problem stays a hypothesis until the first test Counter-evidence: many parking disputes are about entitlement (who is eligible), which a fair draw doesn't fix; some societies simply trust their committee Missing voices: committee members who run draws; residents who never complain online

| Type | Claim | Strength | Speaker | About | Source | Date |
|---|---|---|---|---|---|---|
| assumption | (lead, page not opened: search summary) Owners in a Hyderabad gated community rejected the builder's parking allotment as a 'rigged lottery' with no declared procedure | moderate | reporter | residents of one gated community | https://www.deccanchronicle.com/nation/someday-our-emis-will-cover-our-dignity-888733 | 2026-10 (accessed) |
| assumption | (lead, page not opened: search summary) A Pune-area society of 45 members allotted parking by lottery; columns describe cooperative-court complaints over parking under the bye-laws | moderate | columnist from a housing-law NGO | readers' societies | https://www.moneylife.in/article/housing-society-problems-and-solutions-parking-rules-maintenance-dues-and-agm-participation/81419.html | 2025-08 |
| assumption | (lead, page not opened: search summary) Advocates answering residents: drawing lots is the accepted method when members outnumber slots; a draw where not all members were called is improper; challenges go to the deputy registrar | moderate | advocates | residents' cases | https://www.kaanoon.com/224859/parking-allotment-issues | 2026-10 (accessed) |
| assumption | Residents present at an AGM will scan a QR and tap once from their own phone | weak |  |  |  | 2026-10 |
| hypothesis | At least 3 of 10 Pune society residents asked have seen a parking or amenity allotment contested in their society in the last 3 years | weak |  |  |  | 2026-10 |

### Competitors and alternatives

| Class | What the scan found |
|---|---|
| startup | MyGate and NoBrokerHood run society apps (MyGate has a rent-a-slot market); NoBrokerHood describes lotteries as a manual practice |
| oss | commit-reveal and VRF raffle contracts are common examples |
| hackathon | SponsoredRaffle (Gelato VRF raffle) and FairDrops (skill-game giveaways) in past Blitz repos; neither commits an eligibility list or takes randomness from the room |
| platform | RANDOM.ORG third-party draws for businesses |
| research | commit-reveal randomness beacons (RANDAO) are well known; last-revealer bias is the known weakness |
| workaround | paper chits in a bowl at the AGM, then complaints to the registrar |

| Name | Kind | URL | How this differs |
|---|---|---|---|
| RANDOM.ORG third-party draws | platform | https://www.random.org/draws/ | the committee still chooses when to run and whether to re-run; residents can't contribute or recompute |
| SponsoredRaffle (Monad Blitz) | hackathon | https://github.com/monad-developers/monad-blitz-delhi | oracle VRF raffle for giveaways; no frozen eligible list, no in-room contribution, no allocation of named slots |

### Sponsors

| Sponsor | Product | Capability | Why needed | Without it | Component | Depth | Sponsor goal |
|---|---|---|---|---|---|---|---|
| Monad | Monad testnet | sub-second blocks, near-free transactions, EVM | about 100 residents' entries and reveals plus the draw must settle within a minute in a live meeting, gas-sponsored | on a 12 s chain the reveal window and draw take minutes and sponsoring every resident costs real money; on a database the committee can re-run privately | the draw contract and the live contribution round | core | shows Monad's speed doing something people feel: a whole room transacting at once and getting a checkable result in a second |

### Judging criteria alignment

| Criterion | How it's earned | Basis | Source | Gap |
|---|---|---|---|---|
| Novelty & originality | turns the room into the randomness source for a real Indian society problem | inference | https://github.com/monad-developers/monad-blitz-london | raffles exist; must lead with allocation + frozen list |
| Innovative mechanics on Monad | 100-person commit-reveal round in under a minute, live block ticker on the projector | official | https://github.com/monad-developers/monad-blitz-london | testnet RPC limits unverified |
| Problem-solving | society parking draws contested as rigged (E13-E15) | inference | https://www.moneylife.in/article/housing-society-problems-and-solutions-parking-rules-maintenance-dues-and-agm-participation/81419.html | entitlement disputes out of scope |
| Learning & experimentation | explain last-revealer bias and how the post-close block hash removes it | pattern | https://github.com/monad-developers/monad-blitz-london | none |

### Demo

- **First 10s:** Every Pune society has had this fight: the parking draw. Chits in a bowl, and half the building says it was rigged.
- **First minute:** QR on screen; the room joins as residents of a fictional society; entries land on the projector block by block
- **Wow moment:** host closes entries, phones reveal, the allocation lands in about a second; then every phone taps Verify and shows a match; the host clicks Draw again and it reverts on-chain
- **Technical depth:** a 20-second card: frozen list hash, N reveals, block hash after close, seed, shuffle; why the last revealer can't bias it
- **Story:** the Moneylife and Kaanoon cases of contested draws and registrar complaints
- **Unhappy path:** re-run reverts; late joiner after close is refused; a phone that went to sleep still counts (its commit stays in)
- **Judge touches:** their own phone: join, tap, result, verify

| Criterion | Moment that earns it |
|---|---|
| Innovative mechanics on Monad | block ticker with 80 entries landing in seconds |
| Problem-solving | first 10 s hook |
| Novelty & originality | the room is the randomness |
| Learning & experimentation | the seed card |

**Closest past winner:** {"name": "none found", "searches": ["Monad Blitz winners (11 searches, see judging-intel.md)"]}

**Outsider test:** Script: 'In a society parking draw, the committee can re-run it or leave people off the list. Parchi freezes the list in public, everyone in the room adds randomness from their phone, and anyone can check the result. Would your society use this?' Not yet run with a person.

### Anti-pattern checks

| Check | Hit | Why |
|---|---|---|
| no_clear_user | False | AGM resident and committee member |
| nonexistent_problem | False | E13-E15 from three organisations |
| technology_first | False | problem found by blind discovery before the chain was considered for it |
| clone | False | no society draw tool with frozen list + room randomness found |
| recycled_generic | False | not on the recycled list |
| default_match | False | D9 match declared with a research-backed wedge |
| ledger_repeat | False | first run |
| chatgpt_wrapper | False | no model |
| generic_rag | False | no model |
| generic_agent | False | no agent |
| generic_dashboard | False | it produces the allocation |
| llm_wrapper | False | no model |
| no_product_without_model | False | no model |
| prompt_moat | False | no prompt |
| textbox_ui | False | QR join and tap |
| chatgpt_obvious | False | generic raffle is obvious; the society allocation with frozen list is not the obvious output |
| sponsor_first | False | problem first |
| default_entry | False | raffles are ~1 of 67 Indian entries; the angle is allocation with room randomness |
| wrong_brief | False | consumer app on Monad |
| impossible_demo | False | the room itself is the society |
| unrealistic_data | False | the room's own entries; simulated residents labelled |
| resume_first | False | ideas chosen before background was read |
| prior_project_variation | False | no past draw or allocation project |
| forced_blockchain | False | the dispute is that a private database can be re-run; a public record is the fix |
| forced_ar_vr | False | none |
| forced_iot | False | none |
| forced_multi_agent | False | none |
| superficial_sponsor | False | speed and fees are what make the live round possible |
| platformmaxxing | False | one contract |
| weekend_clone | True | a contract and a web app; the hard part (fair seed, relayer, verifier) is shown in the demo |

## Tradeoffs (no scores; the user decides)

| Dimension | Parchi | doing nothing |
|---|---|---|
| evidence strength | moderate, three organisations, read as search summaries | paper chits keep working where committees are trusted |
| demo risk | depends on venue network and testnet RPC; simulated residents as fallback |  |
| build complexity | one contract, one relayer route, five screens |  |

## Sponsor verdicts

| Sponsor | Verdict | Why |
|---|---|---|
| Monad | core for Parchi | the live in-room randomness round and the instant draw rely on sub-second blocks and near-free transactions; the public record is what makes the result checkable |

## Why there aren't more ideas

One survivor. No number was asked for; three other solutions passed the wrapper filter but were cut at the shortlist for a 3-minute live room demo (event wallet: value only shows over a whole event; deposit locker and slot hand-off: the chain adds convenience, not necessity). Nothing died on padding.

## Kill log

| Name | Stage | Reason |
|---|---|---|
| Float-free event wallet | shortlist | strongest evidence (festival refunds, stallholders unpaid) but the value only shows over a whole event and needs a rupee on/off-ramp custodian; a 3-minute room demo can't show the before/after (impossible_demo risk); Indian evidence is a hypothesis |
| Deposit locker with timeout refund | shortlist | speed is convenience, not necessity; operator adoption is the whole risk; demo is a single escrow |
| Pickup slot hand-off | shortlist | a database plus UPI does it; earlier products failed on onboarding, not fees |
| No-show deposits for free meetups | prior-art | crowded: Kickback, Unlock, Kleek, TOP-G |
| NFT tickets against resale fraud | validation | tickets are issued off-chain so the app can't verify them; default NFT-ticketing idea |
| On-chain chit fund | checks | regulated under the Chit Funds Act; speed adds nothing |
| Friend bets on cricket | checks | online money games banned in India from 1 May 2026 |
| Micro-tipping | validation | UPI P2P free and zero MDR up to Rs 2,000 remove the fee argument |
| AI committee assistant | wrapper | reduces to 'give the bye-laws to a model, get a notice back' |
| UPI screenshot reconciler | wrapper | reduces to 'give screenshots to a model, get a table back' |

## Pre-registered defaults (filtered out unless a wedge was proven)

- D1: On-chain casual game (clicker, flappy bird, tap-to-mine, on-chain chess)
- D2: Creator tipping / "buy me a coffee" with crypto, per-second streaming tips
- D3: Prediction market on cricket / IPL / elections among friends
- D4: NFT event ticketing with anti-scalping resale caps
- D5: Social tokens / creator coins / friend.tech clone
- D6: Bill splitting / shared expenses settled on-chain (Splitwise on-chain)
- D7: On-chain chit fund / ROSCA / committee
- D8: Pay-per-second streaming (video, music, Wi-Fi, parking)
- D9: On-chain lottery / raffle / giveaway with VRF
- D10: Loyalty points as tokens for local shops
- D11: Real-time multiplayer on-chain game (agar.io / tank battle / trivia with stakes)
- D12: Crowdfunding / donation tracking with transparency
- D13: Decentralised social feed / on-chain likes and reactions (pay-per-like)
- D14: AI agent wallet that pays for things for you
- D15: Gasless onboarding / social-login wallet "for normies"

## Research gaps

- research_unavailable is set because no web page could be opened from this container (egress policy); web search summaries were available and are used only as leads
- WebFetch and curl were blocked in this container: every evidence item is the search tool's summary of a page ('as returned'), not an opened page
- No Monad Blitz winner could be identified; no winner base rates
- Monad testnet block time, prevrandao behaviour and RPC rate limits not confirmed from docs (docs.monad.xyz blocked)
- Pune V3's own rules page and voting form not seen; criteria come from a London participant's transcription of the shared Blitz brief
- Dates missing for E13 (Deccan Chronicle) and E15 (Kaanoon)

# Discovery: consumer dApp on a fast, cheap EVM chain (quick depth)

## Requester information found in context (ignored)

- The session context included the user's email address (system-attached). I did not use it, search for it, or infer anything from it.
- The working directory name and a `brief.md` already in this output folder name the event, its hosts and the chain. I opened `brief.md` before I noticed this. I did not use it to choose problems, and none of my searches targeted the event, its project gallery, its sponsors or the chain. The capability envelope below is the de-branded one from my brief (`how: user`).
- `prod-idea/ledger.jsonl` does not exist, so this run has no earlier fingerprints or seeds to rotate away from.

## Method and limits

- I wrote `defaults.md` (15 ideas) before any search.
- WebFetch is blocked. The one fetch I tried (portaldaqueixa.com) was refused, and I treat that as final. Every evidence item below is therefore **as returned by the WebSearch summarizer, not opened**. That is weaker than opening the page. Grades are shown nominally, and every "validated" verdict is **provisional (as returned)**. Load-bearing quotes need opening before anyone repeats them in public.
- Budget: 26 of about 30 searches used. The full list is in `research-log.md`.
- Two cross-cutting facts shape every idea here:
  1. **India banned online money games from 1 May 2026.** The Act was passed in August 2025, and the final rules were notified on 22 April 2026 and took effect on 1 May 2026. Only e-sports and online social games are allowed; registration for social games is voluntary ([india-briefing](https://www.india-briefing.com/news/india-online-gaming-regulation-2026-what-operators-must-know-44355.html), [storyboard18](https://storyboard18.com/gaming-news/online-gaming-rules-eased-social-games-registration-voluntary-under-prog-act-2025-ws-e-93213.htm), as returned). Any stake, bet or paid-entry prize game for Indian users is out.
  2. **In India, small payments are already free.** P2P UPI is free. Merchant (P2M) payments up to ₹2,000 carry zero MDR, and small merchants receiving up to ₹1 lakh a month pay zero MDR at any ticket size. The framework takes effect on 15 Oct 2026 ([newsonair, 2026-09-17](https://newsonair.gov.in/p2p-upi-transactions-free-zero-charges-for-vendors-earning-upto-1-lakh-month-via-upi/), [uniindia, 2026-09-15](https://www.uniindia.com/business-economy/business-upi-mdr-merchant-deals/180767), as returned). **"Micro-payments too small for UPI fees" is false in India.** So in India the chain has to earn its place through custody, programmable conditions, atomicity or verifiability, not through low fees.

## Defaults (from `defaults.md`)

D1 on-chain casual game · D2 creator tipping/streaming tips · D3 cricket prediction market · D4 NFT ticketing with resale caps · D5 social tokens · D6 on-chain bill splitting · D7 on-chain chit fund/ROSCA · D8 pay-per-second streaming · D9 VRF raffle/giveaway · D10 loyalty tokens · D11 real-time multiplayer game · D12 crowdfunding escrow · D13 on-chain likes · D14 AI agent wallet · D15 gasless onboarding.

What research did to them: D3 is illegal for Indian users (fact 1 above). D2 and D8 lose their fee argument in India (fact 2). D7 is legally exposed (P6). D4 is crowded and depends on the issuer (P5). D9 appears only with a research-backed wedge (S2a).

## Audience map (their own words)

| Group | How they describe themselves and their jargon | Watering holes searched |
|---|---|---|
| Festival-goers and stallholders (EU, UK, Chile, Canada) | "cashless wristband", "top-up", "refund window", "leftover balance", "stallholder", "traders", "pitch fee", "takings", "ring-fenced" | consumer-body complaint lists (OCU, Test-Achats, Portal da Queixa), Trustpilot, regulator (SERNAC), trade body (NCASS), trade press |
| Indian housing-society residents | "society", "CHS", "managing committee", "AGM", "stilt parking", "allotment", "draw of lots", "bye-laws", "registrar" | Moneylife columns, Kaanoon legal Q&A, Deccan Chronicle, Outlook Money, NoBrokerHood and MyGate pages |
| Pickup-sport players and organisers (UK, India, SG, US) | "5-a-side", "turf", "box cricket", "drop out", "no-show", "match fee", "sub", "block booked", "split bill" | FootyAddicts game listings, Meetup groups, Playo/Squadd/Playtomic pages, twentytwo13.my, Kasa Kai |
| Indian renters of bikes and rooms | "security deposit", "refund not received", "being checked", "co-living", "self-drive" | consumercomplaints.in, Inc42 |
| Indian fans (concerts) | "black tickets", "resale", "fake ticket", "Insta seller" | news (ED raids, FIRs) |
| Informal savings groups (IN/PK) | "BC", "committee", "kitty", "chit", "foreman" | news, Chit Funds Act text |
| Meetup and community-event organisers | "RSVP", "no-show", "refundable deposit", "waitlist" | Meetup group rules, BangPypers mailing list |

---

## Problem table (all problems, kept and killed)

Strength is graded per §Evidence, as returned. "Org-independent" means at least two different organizations are speaking.

| # | Problem (user · moment) | Lenses (trigger first) | Evidence strength | Status | Kill reason / note |
|---|---|---|---|---|---|
| **P1** | **Festival-goers lose leftover cashless balances (short refund windows, fees, lost wristbands), and stallholders' takings sit in the organiser's account until the organiser pays out, or doesn't** · after the event | 3 bad incumbents, 2 money spent on a worse fix, 5 newly possible | **Strong (provisional)**: regulator action (SERNAC 2019 collective procedure; SERNAC letter Oct 2025), trade-body boycott advice (NCASS), complaints in ≥4 independent venues, a 2025 vendor case (Edmonton) | **KEPT** | India segment (college-fest coupons) is neighboring and weak: one organiser account from ~2014 |
| **P2** | **Indian housing-society residents distrust committee- or builder-run draws that allocate scarce parking or amenities** · at allotment and the AGM | 1 painful workflow, 3 bad incumbents, 7 underserved | **Moderate (provisional)**: news report of a "rigged lottery" (Hyderabad), Moneylife columns (Pune-area lottery; Aug 2025), several advocates on Kaanoon, a 2026 appellate-court order on a parking dispute | **KEPT** | Counter-evidence: many disputes are about entitlement, not the draw's honesty |
| **P3** | **Pickup-game organisers absorb the pitch or turf fee when players drop out late** · the 24 h before kickoff | 1 painful workflow, 2 money spent | **Moderate (provisional)**: organisers' own written rules in two UK groups (forward payment, "liable unless replaced", bans); SG and US groups' accounts; one Indian turf-meet organiser separates interest from paid tickets | **KEPT (weak chain fit)** | Prior art: Squadd turf split-bill, Playtomic split payments. Indian evidence is thin |
| **P4** | Organisers of free community events (meetups) face 10–30% RSVP no-shows | 1, capability lens | Moderate: Meetup group deposit rules, The Indian Thing's no-show policy, BangPypers thread | **KILLED: `crowded` + why-now fails** | Kickback/BlockParty, Unlock "I'm Going!", and the Devfolio projects Kleek and TOP-G already do deposit-and-forfeit. Kickback's blocker was **onboarding, not fees** ([hackmd notes](https://hackmd.io/@vrde/SJZG0NjKS)), so a fast chain doesn't remove it. Counter-evidence: BangPypers says asking for money "doesn't look good" for a mature community |
| **P5** | Indian concert fans buy fake or black-market tickets from Instagram, WhatsApp or Telegram sellers | 3 | **Strong**: ED searches at 13 locations in 5 cities (Oct 2024), FIRs by BookMyShow, fans turned away at the gate | **KILLED: no buildable slice + `default_entry` (D4)** | Tickets are issued off-chain by the ticketing platform, so a fan-side dApp can't verify them. The issuer-side fix is NFT ticketing (D4, crowded). Scalping is reported as illegal in India |
| **P6** | BC/committee (ROSCA) members lose savings when the organiser absconds | 1, 7 | Moderate: Karachi scheme of Rs 420m (Dec 2022); Phagwara case (undated) | **KILLED: legal + default (D7) + speed irrelevant** | Chit Funds Act s.4 bars running a chit without state sanction and registration ([Act PDF](https://financialservices.gov.in/beta/sites/default/files/2022-10/4.%20THE%20CHIT%20FUNDS%20ACT,%201982.pdf), as returned). Monthly cadence, so speed adds nothing |
| **P7** | Friend groups and ex-fantasy players want stakes on cricket after the ban | 4 forced change | Fact (law) only, no behavior item | **KILLED: illegal for Indian users** | Online money games banned from 1 May 2026. A free social version has no chain necessity |
| **P8** | Instagram giveaway entrants suspect rigged winners | 3 | Weak: sources describe phishing and fake accounts, not rigging | **KILLED: weak evidence + default (D9)** | — |
| **P9** | Tips and service charges don't reach restaurant staff (India) | 4 forced change | Weak: the Delhi HC ruled service charge voluntary (Mar 2025); CCPA fined Bora Bora Rs 50,000 (Dec 2025); the "staff distribution" point is only an industry argument | **KILLED: hypothesis, no pain evidence** | — |
| **P10** | Indian small merchants and creators need micro-payments cheaper than UPI | capability lens | Counter-evidence (fact 2) | **KILLED by counter-evidence** | Zero MDR ≤ ₹2,000 and for small merchants |
| **P11** | Indian turf/box-cricket groups split the booking fee and chase friends on UPI | 1 | Weak: Squadd FAQ (a product exists) | **MERGED into P3, then `forced_chain`** | UPI is free and instant; Squadd already does split-bill. Chain = database |
| **P12** | Renters of bikes or co-living rooms wait weeks, or forever, for refundable deposits held by small operators | 3, 7 underserved | **Moderate (provisional)**: consumercomplaints.in (Deyor, ₹8,000, July 2025; Rana Cabs, ₹40,000, 2023), Inc42 (Settl tenants), ofo deposit-refund queues (China, 2018, precedent) | **KEPT** | Adoption risk: the operator must agree to lock deposits it now holds |

Lens balance: lens 3 triggered 4 of 12, lens 1 triggered 4 of 12, forced change 2 of 12 (at most a third ✓), capability lens 1 of 12 (at most a quarter ✓). Groups with real evidence: festival-goers and stallholders (non-Indian), Indian society residents, pickup organisers (UK and India), Indian renters, Indian fans. Kept: P1, P2, P3, P12.

---

## Evidence ledger

`how` = as returned by the WebSearch summary (not opened) unless marked. Dates are publication dates where the summary showed them; otherwise "date not shown" or the access month.

| ID | Claim (paraphrased; quotes are as returned) | URL | Date | Speaker | About | Segment | Kind / strength |
|---|---|---|---|---|---|---|---|
| E1 | Online money games banned; only e-sports and social games allowed; rules notified 22 Apr 2026, in force 1 May 2026; social-game registration voluntary | india-briefing.com/news/india-online-gaming-regulation-2026-what-operators-must-know-44355.html ; storyboard18.com/gaming-news/online-gaming-rules-eased-social-games-registration-voluntary-under-prog-act-2025-ws-e-93213.htm ; visionias.in/blog/current-affairs/government-notifies-promotion-and-regulation-of-online-gaming-rules-2026 | 2026 | secondary summaries (law firm and news) | Indian law | — | Fact (rule; Gazette not opened) |
| E2 | Indian merchant payments ≤ ₹2,000 at zero MDR; small merchants (≤ ₹1 lakh/month) at zero MDR whatever the ticket size; P2P free; effective 15 Oct 2026 | newsonair.gov.in/p2p-upi-transactions-free-zero-charges-for-vendors-earning-upto-1-lakh-month-via-upi/ ; uniindia.com/business-economy/business-upi-mdr-merchant-deals/180767 | 2026-09-15/17 | public broadcaster; news agency | Indian payments | — | Fact (counter-evidence) |
| E3 | Belgian buyer refused a cashless refund because of a "strict 3-week refund window" they were "never informed" of | test-achats.be/plainte/plaintes-publiques/refus-de-remboursement/6e531c930c4a6c55fa | date not shown | complainant (consumer body's public list) | own case | same (festival-goer) | Evidence, moderate (first-person, past tense) |
| E4 | Spanish buyers report wristband refunds never received; repeated unanswered contact | ocu.org/reclamar/lista-reclamaciones-publicas/no-reembolso-dinero-pulsera-ca/6fc8ee1315e6cecbf9 (+3 more OCU entries) | date not shown | complainants on OCU | own cases | same | Evidence, moderate |
| E5 | Portuguese complaints: Kalorama cashless refund; Reggae Sun Fest "difficulty refunding balance and costs not communicated" | portaldaqueixa.com/brands/kalorama/complaints/kalorama-reembolso-cashless-2-80181322 ; portaldaqueixa.com/…/reggae-sun-fest-…-161210826 | date not shown | complainants | own cases | same | Evidence, moderate. **Fetch refused (egress blocked)** |
| E6 | Festivals' own pages list refund fees and limits: Boombastic €2.50 fee; Anjunadeep €1.50 fee plus an activation fee; Cavendish $5 fee and $10 minimum balance; Woodford director: no access to credit if the wristband was discarded | boombasticfestival.com/en/cashless ; explorations.anjunadeep.com/cashless ; cavendishbeachmusic.com/?p=2410 ; musicfeeds.com.au/?p=313563 | 2026-10 (accessed) | organisers | their own policies | same | Fact (structure of the loss) |
| E7 | Chile's SERNAC opened a collective procedure against Lollapalooza Chile's organiser and ticketer for not refunding wristband balances (CLP 34 million); case listed closed, update 23 Jan 2026 | sernac.cl/portal/604/w3-article-56863.html ; eldinamo.cl/actualidad/2019/08/19/lollapalooza-pulseras-cashless-punto-ticket-sernac/ | 2019-08; update 2026-01-23 | regulator | attendees | same | Evidence, **strong** (regulator enforcement) |
| E8 | SERNAC contacted the Creamfields Chile 2025 producer after complaints about unrefunded cashless balances | chocale.cl/2025/10/tras-fallas-de-seguridad-y-otros-problemas-sernac-oficio-a-la-productora-de-creamfields-2025/ | 2025-10 | news, reporting regulator action | attendees | same | Evidence, strong (regulator action, recent echo) |
| E9 | Galtres Parklands: about £120k owed to ~40 stallholders after a cashless festival; NCASS advised members to avoid cashless work "until a system for ring-fencing this money is developed"; blamed organisers who "failed to hold the funds in trust" | news.pollstar.com/2014/10/01/galtres-is-cashless/ ; bigissuenorth.com/news/2015/07/festivals-row-with-creditors/ ; accessaa.co.uk/cashless-payment-code-of-conduct-proposed-following-festival-controversy/ | 2014-10; 2015-07 | trade body NCASS; trade press | stallholders | same (stallholder) | Evidence, strong (trade-body action, documented losses), **old: needs an echo, see E10** |
| E10 | Edmonton International Beer Festival 2025: two vendors "out thousands" after not receiving their share of revenue that ran through the organiser's ticketing system | ctvnews.ca/edmonton/article/vendors-say-beer-festival-still-owes-them-from-last-years-event/ | 2025–26 | news quoting vendors | vendors | neighboring (ticket revenue share, not cashless) | Evidence, moderate |
| E11 | NIT Trichy Festember: RFID cash cards for 4,000+ guests; Pragyan: reusable food cards "drastically reduced" operating costs and "the problem of counterfeit coupons" | blooloop.com/technology/news/semnox-provides-cashless-solution-for-pragyan-tech-fest/ ; blooloop.com/technology/news/semnox-parafait-cashless-debit-card-system-success-at-festember2014-nit-trichy/ | ~2014 | vendor press via trade site | organisers | neighboring (Indian fest) | Evidence, weak (vendor PR; old) |
| E12 | UNTOLD (Romania) pilot: WalletConnect Pay stablecoin checkout in the VIP area, 6,000 users; no outcome data found | walletconnect.com/blog/rhuna-ingenico-and-walletconnect-bring-stablecoin-and-crypto-checkout-to-untold-festival | date not shown | vendor | own pilot | — | Prior art (weak; vendor claim) |
| E13 | Hyderabad gated community: owners rejected the builder's parking allotment as a "rigged lottery"; no declared procedure | deccanchronicle.com/nation/someday-our-emis-will-cover-our-dignity-888733 | date not shown | news | residents | same | Evidence, moderate |
| E14 | Moneylife columns: a Pune-area society of 45 members allotted parking by lottery; a member parked 3 vehicles on 1 slot; Aug 2025 column on cooperative-court complaints under bye-law 174(B)(iv) | moneylife.in/article/housing-society-problems-and-solutions-parking-rules-maintenance-dues-and-agm-participation/81419.html | 2025-08 (one column) | columnist (housing-law NGO) | readers' societies | same | Evidence, moderate (specialist describing cases) |
| E15 | Kaanoon advocates: drawing lots is the accepted method when members outnumber slots; challenges go to the deputy registrar; a draw where "not even all members were called" is improper | kaanoon.com/224859/parking-allotment-issues ; kaanoon.com/131909/parking-issues-with-society-management-committee ; kaanoon.com/231186/no-car-parking-alloted-to-new-flat-owners | date not shown | advocates answering residents | residents' cases | same | Evidence, moderate (law-firm-style specifics) |
| E16 | Maharashtra State Co-op Appellate Court (2026 interim order): parking is controlled by the society; a Santacruz West dispute over 22 spaces and 18 members | outlookmoney.com/news/new-flat-owners-cannot-automatically-claim-existing-parking-slots-maharashtra-court-rules | 2026 | news on a court order | society members | same | Evidence, moderate (litigation effort). Counter: about entitlement, not draw honesty |
| E17 | NoBrokerHood blog describes parking lotteries as a common, committee-run practice; MyGate's documented parking feature is "Rent a Parking" (marketplace), and no draw feature was seen | nobrokerhood.com/blog/parking-lottery-system/ ; help.mygate.in/articles/129396-what-is-rent-a-parking-feature-on-the-mygate-app | 2026-10 (accessed) | incumbents | their features | — | Prior art. Absence claim: none of the 2 incumbents' pages seen shows a verifiable draw |
| E18 | RANDOM.ORG sells third-party certified draws ($4.95 up to 500 entries … $1,149.95 for 1M) so a business can "refute any accusations of tampering" | random.org/draws/pricing ; draws.random.org | 2026-10 (accessed; page © 1998–2024) | vendor | its service | neighboring (businesses, not residents) | Evidence, moderate at most (a priced worse fix: a trusted third party) |
| E19 | Tolworth 5-a-side organisers: "after being let down" they require forward payment from newcomers; players dropping out with < 24 h notice are "liable for the match fee unless we can find a replacement"; £7.68/head "pro-rata increase if less players attend" | footyaddicts.com/football-games/65947-5-a-side-football-goals-soccer-centres-tolworth-greater-london | listing pages from 2019 | organiser | own group | same | Evidence, moderate (self-built rules) but **old** |
| E20 | London pick-up game: 3 days' notice, "black mark system", "2 game bans for no shows"; pitch block-booked | footyaddicts.com/football-games/37-5-a-side-football-new-city-college-poplar-greater-london | date not shown | organiser | own group | same | Evidence, moderate |
| E21 | Singapore walking football: first session nearly cancelled after a cascade of 5 withdrawals the night before | twentytwo13.my/walking-football-didnt-get-off-to-running-start-but-singapore-organiser-unfazed/ | date not shown | news quoting organiser | own group | same | Evidence, moderate |
| E22 | Kasa Kai (turf meets in Mumbai, Gurugram, Bengaluru): sign-up "does not constitute a ticket", so interest and paid confirmation are kept separate | kasakaimumbai.gumroad.com/l/turf-football-meets-in-mumbai-and-other-indian-cities | 2026-10 (accessed) | organiser | own meets | same (India) | Evidence, weak–moderate (workaround) |
| E23 | Squadd: turf "Split Bill", where each player pays a share by UPI or card to confirm; Playtomic: split-payment bookings | squadd-web.onrender.com/faq ; helpmanager.playtomic.com/hc/en-gb/articles/50423411876881 | 2026-10 (accessed) | vendors | own features | — | Prior art |
| E24 | Meetup groups: deposits refunded "within 72 hours after the event upon verified attendance"; The Indian Thing removes members after 3 no-shows in 365 days; BangPypers: asking for money "doesn't look good" | meetup.com/the-indian-thing/ ; python.org/pipermail/bangpypers/2017-March/011717.html ; intercom.help/mondaygirl/en/articles/9675785-are-meetups-free | 2017; 2026-10 (accessed) | organisers; community members | own groups | same (P4) | Evidence, moderate; BangPypers is counter-evidence |
| E25 | Kickback/BlockParty: deposit-to-RSVP with forfeits shared among attendees; Unlock "I'm Going!" commitment staking; Kleek and TOP-G hackathon projects | decrypt.co/20435/kickback-pays-you-to-attend-virtual-event-on-ethereum ; unlock-protocol.com/blog/unlock-labs-adds--i-m-going--commitment-feature-to-events-onchain-ticketing-platform ; devfolio.co/projects/kleek-67d7 ; devfolio.co/projects/topg-0fb7 | various | vendors and builders | own products | — | Prior art (P4 crowded) |
| E26 | Developer notes on Kickback: "the transaction fee and speed are not a big hurdle for them. Their problem is onboarding people that don't hold cryptocurrency" | hackmd.io/@vrde/SJZG0NjKS | ~2019 (date not shown) | developer's informal notes | Kickback team | — | Counter-evidence for the fee/speed why-now (anecdotal) |
| E27 | ED searches at 13 locations in 5 cities over Coldplay and Diljit ticket black-marketing, with fake tickets sold through Instagram, WhatsApp and Telegram; fans turned away at the gate | onmanorama.com/news/india/2024/10/26/ed-probe-black-market-concert-tickets-cold-play-dilijit-dosanjh.amp.html ; tribuneindia.com/news/delhi/diljit-dosanjhs-concert-draws-huge-crowd-amid-fake-ticket-scams | 2024-10 | news reporting enforcement | fans | same (P5) | Evidence, strong (enforcement) |
| E28 | Karachi "committee" (ROSCA) collapse, about Rs 420m, mostly women, no written records; Phagwara (Punjab) committee case | thefridaytimes.com/05-Dec-2022/from-kitty-party-to-pity-party-ponzi-scheme-defrauds-hundreds-of-women-of-rs-420m ; tribuneindia.com/news/jalandhar/absconding-man-in-committee-fund-fraud-case-resurfaces | 2022-12; undated | news | members | same (P6) | Evidence, moderate |
| E29 | Chit Funds Act s.4: no chit may be started without state sanction and registration | financialservices.gov.in/beta/sites/default/files/2022-10/4. THE CHIT FUNDS ACT, 1982.pdf ; advocatekhoj.com/library/bareacts/chitfunds/4.php | Act 1982 | statute | — | — | Fact (kills P6) |
| E30 | Deyor Adventures: ₹8,000 bike security deposit not refunded after a trip that ended July 2025; the company says it is "being checked"; escalated to the National Consumer Helpline | consumercomplaints.in/deyor-adventures-non-refund-of-bike-security-deposit-despite-repeated-follow-ups-deyor-adventures-c3537588 | 2025 | complainant | own case | same (P12) | Evidence, moderate |
| E31 | Rana Cabs: deposit promised back "after 15 days" but never refunded; a renter with a Rs 40,000 Royal Enfield deposit says the operator keeps "giving me excuses" | consumercomplaints.in/rana-cabs-b115420 | 2023 (page updated) | complainants | own cases | same | Evidence, moderate |
| E32 | Settl co-living tenants allege delays in security-deposit refunds | inc42.com/buzz/settls-tenants-allege-delay-in-security-deposit-refunds-report/ | date not shown | news | tenants | neighboring (co-living) | Evidence, moderate |
| E33 | ofo users queued for deposit refunds in Beijing after the company ran short of cash | technode.com/2018/12/18/ofo-users-refund-deposits-beijing/ | 2018-12 | news | users | neighboring (precedent) | Evidence, moderate (old precedent) |
| E34 | Delhi HC (Mar 2025): service charge must be voluntary; CCPA fined a Mumbai restaurant chain Rs 50,000 (Dec 2025) | newsonair.gov.in/delhi-hc-rules-restaurants-cant-impose-mandatory-service-charge-in-bills ; storyboard18.com/brand-marketing/hotel-restaurant-bodies-challenge-delhi-hc-ruling-against-mandatory-service-charge-63458.htm | 2025 | court and news | restaurants | P9 | Fact, but no pain item |

---

## Kept problems: validation cards

### P1: event money held by the organiser (festival-goers and stallholders)
**Problem: validated, strong (provisional, as returned) · Wedge: untested**
- **User and moment:** (a) an attendee in the days after a festival, trying to recover leftover credit; (b) a food or drink stallholder after the event, waiting for the takings the organiser collected on their behalf.
- **Job:** get back money that is yours, which the event's payment system holds.
- **Pain and intensity:** fees of €1.50–€5 per refund plus minimum balances (E6); short windows that were never announced (E3); refunds that never arrive (E4, E5); a regulator stepping in twice in Chile (E7, E8). Stallholders lost about £120k across ~40 traders in one case (E9), and vendors were "out thousands" in 2025 (E10).
- **Frequency:** every cashless event; recurring every season (E7 2019, E8 2025).
- **Workaround:** online refund forms within a window; keeping the physical wristband; consumer-body complaints; trade-body boycott advice and a proposed code of practice (E9).
- **Spend:** organisers pay cashless providers (E11 shows an Indian fest bought RFID cards); attendees pay refund fees (E6).
- **Counter-evidence:** some festivals charge no refund fee (Sonar Lisboa, Alcatraz, as returned), so the problem is uneven. NCASS said the cashless provider was not at fault; the issue is the organiser's custody of the float.
- **Who is missing:** Indian attendees. No Indian complaint about cashless balances turned up, so the India segment is a hypothesis.
- **Prior-art probe:** UNTOLD stablecoin checkout pilot (E12), which serves crypto holders' convenience; cashless providers Intellitix, wrstbnd and Semnox (search titles). Gap: none of the products seen ring-fences stallholder takings by construction or removes the refund window (absence claim limited to the pages returned).

### P2: allocation draws for scarce shared assets in Indian housing societies
**Problem: validated, moderate (provisional, as returned) · Wedge: untested**
- **User and moment:** a resident at the AGM or allotment meeting where parking or amenity slots are drawn; the committee member who has to run a draw nobody will accuse them of rigging.
- **Job:** allocate N slots among M > N eligible members in a way every member accepts.
- **Pain and intensity:** draws contested as "rigged" (E13); slots reassigned to committee members without notice (Moneylife, as returned); disputes go to the deputy registrar, a consumer forum or the cooperative court (E14, E15, E16), which costs months and legal fees.
- **Workaround:** paper slips drawn at the AGM; RANDOM.ORG-style paid third-party draws for businesses (E18); litigation.
- **Counter-evidence:** many parking disputes are about entitlement (E16), and a fair draw doesn't fix those. A society may simply trust its committee.
- **Prior-art probe:** MyGate offers a rent-a-slot market, and NoBrokerHood describes lotteries as a manual practice (E17). Crypto VRF raffles exist for crypto-native giveaways (D9), not society allocation.

### P3: pickup-game organisers carry the cost of late dropouts
**Problem: validated, moderate (provisional; UK items old, India thin) · Wedge: untested**
- **User and moment:** the person who block-booked the pitch or turf, in the 24 h before kickoff, as players drop.
- **Pain:** the organiser is "out of pocket"; pro-rata increases for those who turn up (E19); cascading withdrawals (E21).
- **Workaround:** forward payment, a "liable unless replaced" rule, bans and black marks (E19, E20); interest kept separate from paid confirmation (E22); split-bill apps (E23).
- **Counter-evidence:** Squadd and Playtomic already do per-player payment. In India, UPI makes collecting free and instant. The pain left over is the drop-and-replace step.

### P12: renters' refundable deposits held by small operators
**Problem: validated, moderate (provisional, as returned) · Wedge: untested**
- **User and moment:** a tourist or student returning a rented bike or scooter (or leaving a co-living room) and waiting for the deposit.
- **Pain:** ₹8,000 unrefunded for months (E30); a ₹40,000 deposit stuck behind "excuses" (E31); co-living delays (E32). Precedent: ofo spent deposits as working capital and couldn't repay them (E33).
- **Workaround:** repeated follow-ups, the National Consumer Helpline, consumer commission complaints.
- **Counter-evidence:** large operators (Royal Brothers, Bounce) produced no complaints in this search. The operator has no obvious incentive to give up custody.

---

## Solutions (Stage 4), with Stage 4b and the chain-necessity test

Every surviving solution has **no model in its loop** (`ai_in_loop: false`). They pass Stage 4b trivially, and the answers are written out anyway. The real filter here is `forced_blockchain`: what the chain does that a normal database plus UPI or Stripe could not.

### S1a: Float-free event wallet (P1) **SURVIVES (strongest)**
- **Core workflow:** at the gate, an attendee scans a QR and gets a browser wallet with a top-up (on testnet, from a faucet or organiser grant). Each stall has its own address and QR. The attendee scans the stall QR and pays; the contract splits each sale atomically (e.g. 90% to the stall, 10% commission to the organiser) and the stall's live board ticks up within about a second. The unspent float sits in the contract, not with the organiser. The attendee taps "take my leftover back" at any time, with no window or form. The organiser has no function that can withdraw attendees' float.
- **Insight:** both failures in the evidence (stallholders unpaid, attendees unrefunded) come from one structural fact: the organiser holds the float. NCASS asked for exactly this ring-fencing in 2014, and no settled industry fix appeared in the results (E9). A contract makes ring-fencing the default instead of a contract term.
- **Replaces:** the organiser's settlement run to vendors, the refund form and window, and paper coupons.
- **Behavior change:** stallholders accept on-chain payouts; attendees hold a browser wallet for a day.
- **What the fast chain does (chain necessity: STRONG):** neutral custody among three parties who don't trust each other (organiser, stallholders, attendees); per-sale settlement straight to the stall in about 1 s, which a queue at a food stall needs (the ~0.4 s blocks and ~1 s finality in the envelope, `how: user`); sub-cent fees on ₹50–₹200 or €3–€10 items. A database plus a bank "trust account" could do the custody, but only if the organiser sets it up honestly, and the evidence is that they don't. **Honest limit:** top-up (fiat → token) and cash-out still need an on- or off-ramp, which is custodial and, in India, may be a stored-value or PPI question (assumption; not checked).
- **Fingerprint:** domain events and payments · user: stallholder at each sale, attendee after the event · job: get paid, or get leftover back · mechanism: the contract holds the float, splits the commission per sale, refunds on demand · family: transact / prevent · demo: "the audience buys from three stalls on stage, the stall boards update live, the organiser tries to withdraw the float and the call reverts, an attendee pulls back the leftover in 1 s".
- **Default match:** none in the defaults list. Crypto conference payment tokens are a near-default; the wedge is float custody for non-crypto stallholders (E9, E10), not convenience for crypto holders (E12).
- **Why-now object:** `kind: cost/latency (platform)` · `date:` not checked: the de-branded envelope, by design; a research gap · `change:` an EVM chain with ~0.4 s blocks, ~1 s finality and very low fees (`how: user`) · `threshold:` per-sale confirmation ≤ ~1 s at a stall, and fee ≪ 1% of a ₹50 item (assumption) · `who_couldnt_before:` small festivals and college fests that couldn't afford a ring-fenced trust or escrow arrangement or an RFID vendor · `sentence:` "Since <chain GA date, unchecked>, sub-second finality at sub-cent fees lets a small festival settle every stall sale straight to the stallholder and leave the float in a contract, instead of holding it in the organiser's account; earlier fixes (NCASS's 2014 code of practice) failed because ring-fencing depended on the organiser's goodwill (assumption: no evidence of adoption), which a contract removes."
- **Stage 4b.** (1) Product: a per-event stall registry, an attendee wallet, the float contract, live per-stall sales boards and an attendee balance/refund page. (2) AI leverage: none; `ai_in_loop: false`. (3) Without a model: everything remains (contract, ledger, boards) → **substantial**. Reduction test: not applicable; no model.
- **Authenticity signals:** narrowly defined user and moment (stallholder at the sale; attendee after the event) — *evidence* (E3–E10) · existing inefficient workaround (refund forms and windows, trade-body boycott) — *evidence* (E6, E9) · writes into the system of record (the sale *is* the settlement) — *commitment*: the MVP test checks the stall balance equals the sum of its sales minus commission · deterministic core (commission split, a float with no withdrawal function) — *commitment*: a test checks the organiser's withdrawal reverts · accumulated history (per-stall sales history usable for the next event's pitch fees) — *commitment* · trust a platform lacks (no custodian) — *evidence* (E9, "failed to hold the funds in trust").
- **Load-bearing assumptions:** organisers will give up float and breakage income, which is a counter-incentive, so the pressure would have to come from stallholders or regulators; attendees accept a browser wallet; on- and off-ramp legality.
- **Hypothesis:** "At least 3 of 5 food-stall operators at a Pune college fest or flea market say they waited more than a week for an organiser payout, or lost coupon value, in the last year." Test: ask them at the next market.
- **Alternative inside this card (S1b, India college-fest coupons):** tokenized fest coupons that stalls redeem instantly, stopping counterfeits (E11). **`forced_chain` warning:** where the fest committee is trusted and an RFID or QR-coupon vendor exists, a database does the same. Kept only as a segment of S1a, not as a separate idea.

### S2a: Live multi-party verifiable allocation draw (P2) **SURVIVES**
- **Core workflow:** the committee creates a draw with the slots, the eligible members (flat numbers) and any priority rules. The contract commits the eligible list *before* the draw opens, so nobody can be left out or added later. At the AGM each resident present scans a QR and taps to contribute randomness (two quick commit-reveal rounds of about 20 s each). The contract derives the seed from all contributions and runs a deterministic shuffle. Results show instantly. A verifier page recomputes the allocation in the browser from public chain data. Re-runs and list edits revert.
- **Insight:** the disputes in the evidence are about who was included (E15: "not even all members were called") and whether the drawer could bias the draw (E13). A trusted third party (E18) still lets the committee choose when to run and whether to re-run. Making every resident a source of randomness and committing the list first removes both.
- **Replaces:** paper slips at the AGM, a trusted third-party draw, and the complaint to the registrar.
- **What the fast chain does (chain necessity: STRONG for verifiability, MODERATE for speed):** a public record nobody controls that the list and seed were fixed before the result; multi-party randomness. The speed is what makes a 50–100-person live contribution round finish within a minute or two in the meeting room, and near-zero fees let it be sponsored for every resident. A committee-run database can be re-rolled privately, and that suspicion is the whole dispute.
- **Fingerprint:** domain community governance · user: a society resident at the AGM allotment · job: accept the allocation of scarce parking or amenity slots · mechanism: a committed eligibility list, participant-contributed randomness, a deterministic on-chain shuffle and a public verifier · family: verify / prevent · demo: "30 people in the room join a draw for 10 slots, everyone taps, the allocation lands in seconds, the committee's re-run attempt reverts".
- **Default match:** **D9 (VRF raffle) on job and mechanism.** `why_different`: a different user and job (allocating scarce shared assets among neighbours who distrust the drawer, not a promotional giveaway), and the mechanism adds the pre-committed eligibility list that the advocates' objection targets (E15). Randomness comes from the participants, not an oracle. Evidence: E13–E16.
- **Why-now object:** `kind: cost/latency` · `date:` unchecked (envelope) · `change:` sub-second blocks with near-free transactions · `threshold:` 50+ contributions plus the reveal inside about 2 minutes at zero cost to residents (assumption) · `who_couldnt_before:` a society AGM without crypto-holding residents or a paid third party · `sentence:` "Since <unchecked date>, sub-second, near-free transactions let a 60-flat society run a draw where every resident present adds randomness from their phone and anyone can verify the result, instead of paper slips or a paid third-party draw (E18); earlier verifiable-raffle tools served crypto-native giveaways (assumption), not society allocations."
- **Stage 4b.** (1) Product: a draw registry with committed eligibility lists, a contribution flow, the on-chain allocation, a verifier page and a per-society history of draws. (2) AI: none. (3) Without a model: everything → **substantial**.
- **Authenticity signals:** narrowly defined user and moment (AGM allotment) — *evidence* (E14, E15) · existing inefficient workaround (paper draw, registrar complaints) — *evidence* (E15, E16) · domain-specific logic (eligibility list, priority rules such as senior or EV slots) — *commitment*: a test checks that the list commitment blocks additions after open · deterministic core (shuffle derived from the seed) — *commitment*: the verifier recomputes and matches · accumulated history (a society's draw record for later re-draws) — *commitment* · trust a platform lacks — *evidence* (E13).
- **Load-bearing assumptions:** residents will join from their phones in the meeting; committees want to be accusation-proof (RANDOM.ORG's pitch, E18, suggests a market for this); the dispute is about honesty, not entitlement (E16 cuts against this).
- **Hypothesis:** "At least 3 of 10 Pune society residents asked have seen a parking or amenity allotment contested in their society in the last 3 years." Test: ten conversations at or after the event.

### S4a: Deposit locker with timeout auto-refund (P12) **SURVIVES**
- **Core workflow:** the operator lists an item (scooter, bike, camera). At checkout the renter locks the deposit in a contract tied to that rental, and before-photos are hashed on-chain. On return the operator either taps "release", which refunds in about 1 s, or files a claim up to a cap with after-photo hashes inside a claim window. The renter accepts or disputes. If the operator does nothing before the window closes, anyone (the renter) triggers the auto-refund. The operator never holds the deposit.
- **Insight:** in every complaint the operator holds the money and the renter has no lever but follow-ups (E30, E31). ofo shows what happens when deposits become working capital (E33). A contract with a timeout flips the default: silence refunds the renter.
- **What the fast chain does (chain necessity: STRONG for custody and timeout, WEAK–MODERATE for speed):** neutral escrow that neither party controls; an enforceable default refund; an instant refund at the counter, which matches the UPI experience the renter expects. A slow chain could do the escrow, so speed is UX here, not necessity, and I'm saying so. A database can't stop the operator from spending the deposit.
- **Fingerprint:** domain rentals · user: a renter at return · job: get the deposit back · mechanism: contract-held deposit with claim window and timeout auto-refund · family: prevent / transact · demo: "rent, return, refund in 1 s; then the operator goes silent and the timer refunds automatically".
- **Default match:** none (D12 escrow is crowdfunding, a different job).
- **Why-now object:** `kind: cost/latency` · `date:` unchecked · `threshold:` an escrow plus refund costing sub-cent per rental with ~1 s finality, comparable to UPI at the counter (assumption) · `who_couldnt_before:` small rental operators and renters, for whom an escrow agent is too expensive for ₹2,000–₹10,000 deposits (assumption) · `sentence:` "Since <unchecked date>, near-free sub-second escrow lets a small rental shop lock each ₹2,000–₹10,000 deposit in a contract that refunds by default, instead of holding it in its own account; earlier escrow services were priced for large transactions (assumption)."
- **Stage 4b.** (1) Product: a rental registry, per-rental deposit escrow, a photo-hash log, a claim/dispute queue and timeout refunds. (2) AI: none in the MVP. An optional perception step (before/after photo comparison that flags claims with no visible new damage) is a flag only; the contract and the claim window decide. (3) Without a model: everything → **substantial**.
- **Authenticity signals:** narrowly defined user and moment (renter at return) — *evidence* (E30, E31) · existing inefficient workaround (follow-ups, NCH) — *evidence* (E30) · taking responsibility for an outcome (default refund) — *commitment*: a test checks the timeout refund fires · deterministic core — *commitment* · accumulated history (an operator's claim and refund record as a public reputation) — *commitment* · trust a platform lacks — *evidence* (E33 precedent).
- **Load-bearing assumptions:** operators adopt it as a trust badge to win tourists (untested; this is the biggest risk); renters accept a crypto on-ramp for a deposit; Indian legality of holding tokens for deposits.

### S3a: Pickup slot hand-off with atomic refund (P3) **SURVIVES (weakest; chain fit disclosed)**
- **Core workflow:** the organiser creates a game (slots, share, kickoff). A player claims a slot by paying a share into the contract. If they drop out, the slot goes on offer; when a waitlisted player claims it, the dropper is refunded in the same transaction. At kickoff the contract pays the organiser. Unreplaced drops stay paid, which automates the "liable unless replaced" rule (E19).
- **What the fast chain does (chain necessity: WEAK–MODERATE):** an atomic money-for-slot swap between strangers (FootyAddicts and Meetup groups are strangers), with no organiser touching money, and an instant refund. **This is mostly a database with UPI or Stripe for Indian friend groups** (UPI is free and instant, E2; Squadd and Playtomic exist, E23). And Kickback-type products struggled on onboarding, not fees (E26), so the fast chain does not remove the main earlier blocker. Kept only because the slot hand-off step is evidenced and no product seen automates it. Weakest of the four.
- **Fingerprint:** domain amateur sport · user: a player dropping out in the last 24 h, plus the organiser · job: leave without leaving the organiser out of pocket · mechanism: an atomic slot-for-refund swap from the waitlist · family: transact · demo: "two phones: one drops, the other claims, and the refund lands in the same block".
- **Why-now:** `kind: cost/latency` · date unchecked · **step 5 fails partly**: the earlier blocker (onboarding, E26) is not removed by speed → disclosed.
- **Stage 4b:** (1) a game registry, slots, a waitlist, the swap contract; (2) AI: none; (3) **substantial**. Authenticity: user and moment *evidence* (E19, E21); workaround *evidence* (E19, E20); domain logic (liable-unless-replaced) *commitment*; deterministic core *commitment*; workflow effect (the waitlist fills itself) *commitment*.

---

## Stage 4b and forced-chain kill log (regenerated or dropped)

Wrapper kills (generated in brainstorming; all failed the reduction test):

| ID | From | Solution | Reduction sentence | Fair summary? | Verdict |
|---|---|---|---|---|---|
| W1 | P11/P3 | AI reconciler that reads UPI payment screenshots in a WhatsApp group and marks who paid | "The user gives payment screenshots to a model and gets a who-paid list back" | Yes | **Killed `wrapper`**. It never owns the payment, and a UPI collect link removes the need |
| W2 | P12 | AI deposit judge that decides deductions from before/after photos | "The user gives two photos to a model and gets a deduction amount back" | Yes | **Killed `wrapper`** as a standalone. Survives only as an optional flag inside S4a, never the decision |
| W3 | P8 | AI scam detector for Instagram giveaway entrants | "The user gives an account handle to a model and gets a scam score back" | Yes | **Killed `wrapper`** (P8 also killed on evidence) |
| W4 | P2 | AI committee assistant that drafts allotment notices and answers residents | Content generator / chatbot shape | Yes | **Killed `wrapper`** |

Forced-chain and default kills:

| ID | Idea | Why killed |
|---|---|---|
| F1 | On-chain bill or turf split (D6, P11) | `forced_chain`: UPI is free and instant (E2); Squadd and Playtomic do per-player payment (E23); the chain is a database dressed up |
| F2 | Neighbour parking swap or rent market (S2b) | `forced_chain` + crowded: MyGate "Rent a Parking" exists (E17) |
| F3 | Real-time player auction for local cricket leagues | `forced_chain` (virtual points; a real-time database does it) and unevidenced. Dropped |
| F4 | Society WhatsApp group-buy collections on-chain | `forced_chain`: payment links do it. Dropped unevidenced |
| F5 | Kickback-style deposit-and-forfeit RSVPs (S3b, P4) | `crowded` (E25) and why-now step 5 fails (E26: onboarding, not fees) |
| F6 | Tokenized Indian college-fest coupons as a separate idea (S1b) | `forced_chain` warning where the committee is trusted. Folded into S1a as a segment |
| F7 | Micro-tipping or pay-per-use micropayments for Indian users (D2, D8, P10) | Counter-evidence: zero MDR and free P2P (E2) remove the fee argument |
| F8 | Cricket prediction markets or friend bets (D3, P7) | Illegal for Indian users from 1 May 2026 (E1) |
| F9 | NFT anti-scalping tickets (D4, P5) | `default_entry`, crowded, and the real fraud involves off-chain tickets the dApp can't verify |
| F10 | On-chain BC/committee (D7, P6) | Legal (E29), default, and speed adds nothing |

---

## Research gaps (couldn't check)

- **Nothing was opened.** All items are WebSearch summaries; portaldaqueixa.com refused. Quotes above are "as returned" and must be opened before public use. The validation verdicts are provisional.
- The chain's GA date and measured fees and finality were not checked (de-branded by design), so every why-now has an unchecked `date`.
- Indian attendees' cashless-balance complaints: none found (Lollapalooza India, Sunburn and NH7 searches returned nothing relevant).
- Indian pickup-game no-show evidence is thin (one organiser, E22). Playo reviews returned no no-show items.
- Dates are missing for several items (E13 Deccan Chronicle, E15 Kaanoon, E20, E21, E32).
- Whether India's closed-loop stored-value (PPI) rules would apply to an event token was not checked.
- Kickback's actual shutdown reason was not found. E26 is an informal note.
- The UK tenancy-deposit-protection analogue for S4a was not sourced (do not cite).
- Reddit (r/pune, r/bangalore, r/festivals) is blocked by policy. Ask the user to paste threads if needed.

## Research log (summary; full list in `research-log.md`)

26 WebSearch queries (standard mode) and 1 WebFetch (refused: portaldaqueixa.com, egress blocked; final). No GitHub clones were needed. No searches about the event, its sponsors, its gallery or the requester.


# Research log (discovery, quick depth)

- Date run: 2026-10-09.
- Tools: WebSearch only (mode "standard", 26 queries). WebFetch was tried once and refused. No GitHub clones.
- Budget: about 30 searches shared; 26 used.
- Defaults were written to `defaults.md` before query 1.
- Requester and event context: a user email in system context, plus the folder name and `brief.md` naming the event and chain. All ignored for problem choice. No queries about the event, its gallery, its sponsors, the chain or the requester.

## Queries

| # | Query (abridged) | Purpose / lens | What came back (as returned) | Used as |
|---|---|---|---|---|
| 1 | Promotion and Regulation of Online Gaming Act 2025 real money ban, what's allowed, rules notified | forced change (4) | Act Aug 2025; draft rules Oct 2025; final rules notified 22 Apr 2026, in force 1 May 2026; money games banned; social games voluntary registration | E1, kills P7/D3 |
| 2 | box cricket turf booking friends back out, organiser pays, split | painful workflow (1), India | Squadd split-bill FAQ; BookMyBox partial payment; venue no-refund policies | E23 (prior art), P11 |
| 3 | concert ticket resale fraud India, fake tickets via Instagram, police 2025–26 | bad incumbent (3) | ED searches in 13 locations/5 cities (Oct 2024); BookMyShow FIRs; Diljit fans turned away | E27, P5 |
| 4 | Instagram giveaway rigged winner complaint India | bad incumbent (3) | phishing/fake-account scams; no rigging evidence | P8 killed |
| 5 | committee/BC/kitty organiser absconded, women savings | painful workflow (1), underserved (7) | Karachi Rs 420m (2022); Phagwara case; Indore society maintenance fraud (2025) | E28, P6 |
| 6 | organising weekly 5-a-side, chasing players for money, no-shows | painful workflow (1), non-Indian | FootyAddicts organisers' rules (forward payment, liable unless replaced, bans, pro-rata) | E19, E20, P3 |
| 7 | festival cashless wristband refund, unused balance complaint, fee, deadline | bad incumbent (3) | Test-Achats, OCU, Portal da Queixa, Trustpilot complaints; festivals' fee pages; Woodford | E3–E6, P1 |
| — | WebFetch portaldaqueixa.com Reggae Sun Fest complaint | open a primary complaint | **REFUSED (EGRESS_BLOCKED). Final; not retried elsewhere** | logged |
| 8 | college fest food coupons unused, India cashless card refund | India segment of P1 | NIT Trichy Festember/Pragyan RFID cards; counterfeit coupons; no refund complaints | E11 |
| 9 | housing society parking lottery dispute, rigged draw, committee | painful workflow (1), India | Deccan Chronicle "rigged lottery"; Moneylife; Kaanoon advocates | E13–E15, P2 |
| 10 | restaurant tips/service charge not passed to staff, Delhi HC 2025 | forced change (4) | Delhi HC Mar 2025 voluntary; CCPA fine Dec 2025; staff distribution is only an industry argument | E34, P9 killed |
| 11 | Playo review, players don't show up, split cost | P3 India validation | no no-show reviews; Playtomic split payments | E23, gap logged |
| 12 | free meetup RSVP no-show rate, refundable deposit on attendance, India | P4 | Meetup deposit rules; The Indian Thing; BangPypers counter-evidence; 10–30% benchmark (vendor guide) | E24 |
| 13 | Kickback event no-show deposit Ethereum dApp, why shut down, gas | prior art / why-now step 5 | Kickback, Unlock "I'm Going!", Kleek, TOP-G (Devfolio) | E25, P4 crowded |
| 14 | festival food traders not paid, cashless takings withheld | P1 vendor side, spend | Galtres 2014: ~£120k owed to ~40 stallholders; NCASS boycott and ring-fencing; Party in the Park Ipswich | E9 |
| 15 | Lollapalooza India / Sunburn / NH7 cashless refund complaints | P1 India segment | nothing on India; SERNAC Lollapalooza Chile 2019 collective procedure (closed, update 2026-01-23) | E7, gap |
| 16 | online draw app for society parking; MyGate/NoBrokerHood lottery feature | prior-art probe P2 | MyGate "Rent a Parking"; NoBrokerHood blog describes a manual lottery; no draw feature seen | E17 |
| 17 | BookMyShow/District official resale or face-value transfer, India 2025–26 | prior-art probe P5 | only 2024 Coldplay coverage; no official resale feature found (the summary misattributed District's owner; ignored) | P5 kill context |
| 18 | UPI zero MDR small merchants 2025–26, UPI Lite limits | counter-evidence for micro-payments | zero MDR ≤ ₹2,000 and small merchants ≤ ₹1 lakh/month; effective 15 Oct 2026; UPI Lite ₹1,000/₹5,000 | E2, P10 killed |
| 19 | bike rental security deposit refund not received, India complaint | new group (renters), lens 3/7 | Deyor (₹8,000, 2025), Rana Cabs (2023), Settl (Inc42), ofo (2018) | E30–E33, P12 |
| 20 | blockchain token payments at music festivals, stablecoin cashless pilot | prior art / earlier attempts P1 | UNTOLD WalletConnect Pay pilot (VIP, 6,000 users); cashless vendor case studies | E12 |
| 21 | society residents protest parking lottery 2025 Pune/Mumbai/Bengaluru/Gurugram | second source and recent echo P2 | Moneylife Aug 2025; Maharashtra co-op appellate court 2026 interim order | E14, E16 |
| 22 | Kickback Makoto Inoue lessons learned, gas cost | why-now step 5 | BlockParty predecessor; hackmd note: fees and speed "not a big hurdle", onboarding is | E26 |
| 23 | RANDOM.ORG Third-Party Draw Service pricing | money spent on a worse fix P2 | $4.95 ≤ 500 entries … $1,149.95 for 1M; "refute any accusations of tampering" | E18 |
| 24 | Chit Funds Act 1982 s.4, unregistered chit illegal, informal BC | kill check P6 | s.4 requires prior state sanction and registration; informal-group status unresolved | E29 |
| 25 | weekend turf group WhatsApp dropouts, organiser pays, India | P3 India validation | Kasa Kai (signup ≠ ticket); Singapore walking football cascade; NJ group cancels below threshold | E21, E22 |
| 26 | festival collapsed 2024–25, traders owed, cashless takings, NCASS | recent echo P1 | Edmonton beer festival 2025 vendors unpaid; SERNAC–Creamfields Chile Oct 2025; Cornwall collapse 2026 (crew) | E8, E10 |

## Source types in the first ten lookups

Regulatory and legal summaries (1, 10), product FAQ and app pages (2), national news (3, 5, 9), security and vendor blogs (4), community game listings (6), consumer-body complaint lists (7), trade press (8), legal Q&A forums (9). That is more than 3 kinds ✓.

## Not accessible

- portaldaqueixa.com (WebFetch refused, egress blocked). Final.
- Reddit, X, the App Store and Play Store full reviews were not attempted (blocked by policy or unreliable per the recipes).

## Requester information encountered

- The user's email address appeared in system context. Ignored.
- `brief.md` in the output folder names the event, hosts and chain. Read once at the start, then ignored for problem selection. No searches on it.


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

