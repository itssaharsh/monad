# Marshal: Q&A

**"Why not just tell the agent in its prompt never to pay strangers?"**
Do that too, but the prompt is enforced by the same model being attacked. Freysa was told never to release the money and was still talked out of it. AIXBT's attacker was inside the control dashboard, so the prompt didn't matter. A prompt also can't keep a daily total across chats, wait for a human, or hold if the server is compromised. The prompt reduces attempts; the wallet limits the damage. Ours is deliberately gullible to stand in for an agent that's already been fooled.

**"How does the wallet decide?"**
Seven checks in the contract, in order; the first failure decides:
1. agent revoked;
2. zero amount;
3. payee not a listed shop;
4. over ₹3,000;
5. over ₹8,000 today;
6. not enough balance;
7. over ₹1,000, so it waits for the owner.

If all pass, it pays. The replay shows which check fired.

**"Isn't this just session keys / Safe allowances / Zodiac Roles?"**
For the caps and allowlist, yes, and those tools are more flexible. They revert out-of-policy calls, so a blocked attempt leaves no trace. Marshal records the refusal on-chain with a hash of the conversation that caused it, adds a human approval queue and a kill switch, and lets anyone replay why. Closest hackathon work: StableSettle (ETHGlobal), LEASH on Monad; neither keeps a replayable refusal log.

**"Why record refusals instead of reverting?"**
A revert erases the evidence. When an agent misbehaves, the useful question is "what was it told?", and the answer has to survive. Recording costs gas, which is why it makes sense on Monad: about a second and a fraction of a cent per attempt.

**"Can't your server lie about the conversation?"**
It could store a false context. The hash makes that claim tamper-evident: change one character and the hash no longer matches. The money rule doesn't depend on it. Even a lying, compromised server can only call `pay()`, and `pay()` only moves money inside the rules.

**"What if the server or the agent's key is stolen?"**
That key can call `pay()` and nothing else: no withdrawing, no changing rules, no approving. The worst case is spending within the limits on listed shops. The owner revokes it from their phone. The owner key lives on the owner's phone, not on the server.

**"Couldn't the attacker spam refusals to fill the log?"**
Every refusal costs the agent's own gas, so spam costs the attacker's operator money. Auto-revoke after N refusals is on the roadmap. It's off today because the room is attacking on purpose.

**"Why Monad?"**
Every attempt, refused ones included, is a real transaction. With 300 ms blocks it lands on the board about a second after the chat, and fees are low enough that logging every refusal is affordable. On a slow or expensive chain you'd only log successes, and the audit trail disappears.

**"Is the AI real?"**
Yes: Google Gemini (gemini-3.5-flash-lite) with one tool, `pay(to, amount, reason)`. If the model is rate-limited, a labelled scripted stand-in answers so the demo keeps working. The wallet doesn't care which one asks.

**"Is this real money? Mainnet?"**
Testnet and a test rupee token (tINR) only. Moving to mainnet means swapping in a stablecoin and redeploying the same contract. We didn't deploy to mainnet today because it needs real funds.

**"Who pays, what's the business?"**
Anyone giving an AI agent a budget: shopping and booking agents, trading bots, DAO treasuries, agent-to-agent payments. The contract is free and open. Revenue comes from:
- a hosted owner app with approval routing (per-agent subscription);
- audit exports and compliance trails for teams running agent fleets.

Demand signal: Coinbase, Privy and Turnkey are all shipping agent spending policies.

**"What did you build today?"**
All of it, during the event:
- the contract (14 tests, one per rule);
- the agent server;
- the board, phone and owner screens;
- deployment and verification.

The idea doc existed before the event (no code). This fork first held Dibs, built earlier today, and Marshal reuses its RPC relay, burner wallet and live-stream code. We used AI coding assistance. All of this is disclosed in the README.

**"What's next?"**
Simulate a payment before approving it; per-merchant limits; auto-revoke on refusal spikes; an ERC-4337/7579 module so any smart account can use it; mainnet with a stablecoin.

**"What if the owner loses their phone?"**
Today, ownership can be transferred to a new key (`transferOwnership`). Recovery with a second guardian is on the roadmap.
