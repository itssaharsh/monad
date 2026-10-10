# Marshal: research notes (read 10 Oct 2026, 15:20–15:35 IST)

**How these were checked:** this sandbox could not open oecd.ai, theblock.co, arxiv.org or docs.monad.xyz (the fetch tool failed on DNS or got a 403). Titles and quotes below are what the search index returned. **Open each link in a browser before it goes on a slide.**

## Evidence for the hook

| # | Claim | Source (title as indexed) | Status |
|---|---|---|---|
| 1 | Freysa, an AI agent guarding a prize pool, was talked into releasing it (Nov 2024, ~$47k) | [OECD.AI incident 2024-11-29-2e31](https://oecd.ai/en/incidents/2024-11-29-2e31): "AI Bot Freysa Manipulated to Transfer $47K Prize Pool". 13.19 ETH; The Block reported ~$47,316. 481 earlier attempts had failed; the winner redefined `approveTransfer` as being for *incoming* funds. | Matches your claim |
| 2 | AIXBT sent ~55 ETH (~$100k), Mar 2025 | [The Block, 18 Mar 2025](https://www.theblock.co/news/regulation/2025-03-18-ai-crypto-bot-aixbt-lost-eth-hack-unauthorized-dashboard-access-346911): "AI crypto bot AIXBT lost $100,000 worth of ETH after hacker gained unauthorized 'dashboard access'". The maintainer said it was "not a result of agent manipulation". | **Correct the wording.** It wasn't strangers fooling the agent: someone broke into the operator dashboard and prompted the bot with two "malicious replies". Say: *"An attacker got into AIXBT's control dashboard and made it send 55 ETH."* That fits Marshal better: the control plane was off-chain, and a cap in the wallet would have bounded the loss whoever was typing. |
| 3 | MINJA memory injection: 98.2% injection success, 76.8% attack success | [arXiv 2503.03704v2](https://arxiv.org/html/2503.03704v2): "A Practical Memory Injection Attack against LLM Agents" (later versions are retitled "Memory Injection Attacks on LLM Agents via Query-Only Interaction") | Numbers match the indexed intro. Say "in the authors' evaluation". The attacker only needs "queries and output observations". |

## Closest products. All of them already do caps, allowlists or revocable keys.

| Product | What it does | How Marshal differs |
|---|---|---|
| [Coinbase Spend Permissions](https://docs.cdp.coinbase.com/coinbase-wallet/improve-ux/spend-permissions) | A spender can pull a per-period token allowance after one signature | No payee allowlist, no approval tier, no refusal record |
| [Turnkey agentic wallets](https://docs.turnkey.com/solutions/company-wallets/agentic-wallets) | Enclave keys, per-signing caps, human co-approval, instant revoke | Closest on approve/revoke. Its policy sits at the off-chain key layer, and refusals aren't on-chain |
| [Privy agent wallets](https://www.privy.io/agent-wallets) | Policy engine: caps, recipient allowlist, revocable keys, human-in-the-loop | Off-chain policy, no on-chain refusal trail |
| [PolicyLayer](https://www.policylayer.com/compare/coinbase-agentic-wallets/) | A policy proxy on top of Coinbase wallets | The proxy pattern Marshal argues against |
| Safe Allowance Module, Zodiac Roles v2, ZeroDev / Biconomy session keys, MetaMask ERC-7715 *(from memory, not re-checked)* | On-chain allowances, role scoping, session-key policies | More expressive than Marshal. But out-of-policy calls revert or fail validation and leave no trace |
| Blockaid *(from memory)* | Simulates and scans transactions at the wallet or RPC layer | Complementary, not competing. Simulation is on Marshal's roadmap |

**Hackathon prior art (expect builders to have seen these):** [StableSettle](https://ethglobal.com/showcase/stablesettle-iw6kh) (ETHGlobal NY 2026; caps and allowlist that revert "even under prompt injection"), [SpendMate](https://ethglobal.com/showcase/spendmate-wmewx) (AgentKit plus limits), [Agent Vault](https://ethglobal.com/showcase/agent-vault-ngc1n) (Sui), and **LEASH on Monad testnet** (GitHub `ChiJian28/LEASH`, not opened: a vault where the agent spends within session limits the contract enforces). No Monad Blitz entry was found.

## The honest differentiation line

> Caps, allowlists and revocable session keys already exist: Zodiac Roles, ZeroDev, Turnkey, Privy, and hackathon vaults like LEASH on Monad. Marshal adds that a refused payment doesn't revert and vanish. It's written on-chain with a hash of the conversation that caused it, so anyone can replay why it was blocked. It sits next to a live approve/deny queue and an instant revoke, all enforced by the wallet contract rather than by a proxy.

Never say "first". Lead with "refusals you can replay", not with "caps beat prompt injection" (StableSettle and LEASH already showed that).

## Three things a sharp builder will raise (have the answers ready)
1. **"The account will not sign" is loose wording.** The agent's key can sign anything; the wallet contract won't *move money* outside its rules. Use: *"A proxy can be routed around. The wallet can't: money only leaves through its rules."*
2. **The context hash comes from your own server.** A compromised server could log a misleading context. It's a tamper-evident record of what the server claimed, not proof of intent. The guarantee is that funds can't move outside the rules.
3. **Refusal spam.** A refusal that doesn't revert still costs the agent gas, so a compromised agent pays for every attempt it logs. Auto-revoke after N refusals is on the roadmap. It's off today because the room is attacking on purpose.

## Monad facts (docs.monad.xyz via search; the RPC probe was blocked from this sandbox)
- Chain 10143. RPC limits: QuickNode 50 rps (eth_call and eth_estimateGas 25 rps), Ankr 300 per 10 s, monadinfra 20 rps with no batch requests. Plan for ≤20 rps.
- "Monad bills the gas limit, not the gas used." Set explicit gas.
- Minimum base fee is 100 gwei. Ordering is by priority fee.
- **Blocks are 300 ms, full finality 600 ms** (not 0.3–0.4 s / ~0.6 s; 400 ms is out of date). Speculative finality comes after 1 block.
- Reserve balance is 10 MON. A tx that would push an account below it through value transfers lands, but reverts and still pays gas. So keep the operator above 12 MON and move money only in tINR.
- Event repo README: fork `main` with "your project name, a one-liner description". It says nothing about portal deadlines or pre-built code.
- npm today: viem 2.57.4, next 16.4.0. **No new packages are needed:** the model call is a plain `fetch` to an OpenAI-compatible endpoint.
