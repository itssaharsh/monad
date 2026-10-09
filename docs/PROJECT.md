# Parchi: society draws nobody can rig

*Monad Blitz Pune V3 · Saturday 10 Oct 2026 · solo entry*

## The problem
In Indian housing societies, more members want parking slots (and other scarce amenities) than there are slots. The accepted method is a draw of lots at the AGM: chits in a bowl. Draws get contested:
- owners rejected a builder's parking allotment as a **"rigged lottery"** with no declared procedure (Deccan Chronicle, Hyderabad);
- housing-law columns describe Pune-area societies allotting parking by lottery and members taking parking disputes to the cooperative court (Moneylife, Aug 2025);
- advocates answering residents say drawing lots is the accepted method, and that a draw where **"not even all members were called"** is improper; challenges go to the deputy registrar (Kaanoon).

*Honesty note: these pages were read through search summaries because the research container couldn't open them. Treat the problem as a strong hypothesis. Test it by asking 10 people at the event: "Has your society ever had a parking draw someone called rigged?"*

The suspicion always comes from the same place: **whoever runs the draw can quietly edit the list or run it again.** Paper can't prove otherwise. Neither can a society app where the committee types in the result, or a third-party random-number site the committee chooses when to run.

## What Parchi does
1. The committee creates a draw: slots P-01…P-06 and the list of eligible flats. **The list is frozen on Monad before anyone joins.** Its hash is public, so nobody can be added or left out later.
2. Residents in the hall scan a QR code, pick their flat and tap **Drop my parchi**. There's no wallet, no app install and no fees. Their phone makes a throwaway key, signs the entry, and Parchi's relayer pays the gas. The relayer can't fake an entry because the contract checks every signature.
3. Each phone also commits a secret random number. When the committee closes entries, phones reveal their secrets automatically.
4. **Draw.** The seed mixes every resident's secret with the hash of a block that didn't exist when anyone revealed. The contract derives the allocation from that seed: first N get slots, the rest get waitlist ranks.
5. **Verify.** Any phone taps Verify and recomputes the whole draw from public chain data: list hash, reveals, block hash, seed and shuffle. It shows **MATCHES**.
6. **Draw again? Reverted.** A draw runs once, enforced by the contract.

## Why Monad
- **A whole room transacting at once.** 40–100 residents join and reveal within a minute in a live meeting. With sub-second blocks every phone sees its parchi land almost immediately. On a 12-second chain, the same meeting would wait minutes per round.
- **Near-zero fees** make it realistic to sponsor every resident's gas, so nobody needs crypto to take part.
- **A public record nobody controls.** That is the fix for "the committee re-ran it". A private database can always be re-rolled quietly.

## How it works (for the developers in the room)
- `Parchi.sol` is a draw state machine: Open → Revealing → Sealing → Drawn. Joins are EIP-712 signed by each resident's burner key and relayed. Reveals are checked against commits. `draw()` is permissionless once one block has passed after sealing.
- **Seed** = `keccak(XOR of revealed secrets, blockhash(sealBlock+1), drawId)`.
- **Why the last person to reveal can't cheat:** withholding is the classic commit-reveal attack. Here you decide whether to reveal before the block whose hash goes into the seed exists, so you can't compute either outcome. Withheld entries still count in the draw.
- **The allocation is a view function.** A Fisher–Yates shuffle runs over the join order, keyed by the seed. The browser verifier runs the same algorithm, and a parity test checks they agree.
- **Built for a live room:**
  - one cached read endpoint, so 100 phones don't hit the rate-limited public RPC;
  - a relayer with 6 keys and per-key nonce queues;
  - explicit gas limits, because Monad charges the gas limit, not gas used;
  - phones keep their entry locally and retry if the venue Wi-Fi drops.
- **Limits we admit:**
  - flat ownership isn't verified (societies already have a member register);
  - priority rules (EV owners, seniors) aren't built yet;
  - the block producer is the remaining trust assumption;
  - it runs on testnet only.

## What's real vs simulated
| Part | Status |
|---|---|
| Draw contract, entries, reveals, seed, allocation | live on Monad testnet |
| Phone entries during the demo | live, from people in the room |
| "Add 20 residents" button | real transactions from server-made keys, labelled **Simulated residents** (for rehearsal and as a Wi-Fi fallback) |
| Blitz Heights CHS | a fictional society |

## Pitch script (3 minutes, live)
The board is already open with the QR showing. Talk while they scan.

- **0:00** "Every Pune society has had this fight: the parking draw. Chits in a bowl, the secretary's kid picks, and half the building says it was rigged."
- **0:12** "Scan this. For the next two minutes you all live in Blitz Heights. There are 6 parking slots and a lot more of you."
- **0:20** *(parchis landing on the board)* "Each of those is a real transaction on Monad. That's your phone, no wallet. Look at the ticker: forty entries in a few blocks."
- **0:50** *Close entries.* "The list is now frozen on-chain. Nobody can be added and nobody can be left out. That's the first thing every society dispute is about."
- **1:00** "Your phones are now revealing the secret they committed when you joined. That's your randomness, not mine."
- **1:10** *Draw slots.* *(slots fill)* "Done in about a second. Check your phone: you've got a slot or a waitlist number."
- **1:25** "Now tap Verify. Your phone just recomputed the whole draw from the chain: same list, same seed, same slots. **MATCHES.**"
- **1:40** "And if I'm a dodgy secretary who doesn't like the result…" *Draw again.* "Reverted. A draw runs once."
- **1:55** "For the builders: commit-reveal plus the hash of a block that didn't exist yet, so the last person to reveal can't game it. A relayer with signed entries, so nobody needs gas. This only feels instant because Monad blocks are sub-second."
- **2:30** "What's next: priority rules for EV and senior slots, and every society's draw history in one place. Parchi: society draws nobody can rig."

## Q&A prep
1. **"Isn't this just a raffle?"** A raffle picks a winner for a giveaway. This allocates scarce shared slots among neighbours who distrust the drawer. The new parts are the frozen eligible list and randomness from the people in the room.
2. **"Can the relayer cheat?"** It can't forge or change entries, because the contract checks each resident's signature. It could refuse to submit one. Your phone would show that, and you can submit directly.
3. **"Can the last revealer bias it?"** No. The block hash in the seed is from a block that didn't exist when they chose. Withholding just leaves their secret out, and they still stay in the draw.
4. **"Why not VRF?"** You'd be trusting an oracle the committee picks. Here the randomness comes from the room, and anyone checks it. VRF could be added as one more input.
5. **"Why blockchain at all?"** The dispute is that whoever controls the database can re-run the draw. A public record nobody controls is the fix.
6. **"Do residents need crypto?"** No. They scan, pick a flat and tap. Burner keys and sponsored gas handle the rest.
7. **"What stops someone claiming someone else's flat?"** Today, the AGM itself (people are in the room). Next: one-time codes from the member register.
8. **"Indian online-gaming law?"** There's no stake and no money prize. It allocates parking, and online money games aren't involved.
9. **"Scale?"** Tested with 40+ simulated residents. Reads are cached, so 100 phones make about one RPC call per second.
10. **"Business?"** Society apps already charge societies per flat. Parchi is the draw module those apps can't credibly run themselves.

## Video storyboard (product-demo-video, judged profile, about 90 s)
1. **0–6 s:** title card "Society draws nobody can rig."
2. **6–20 s:** the board, with the bowl filling and the block ticker running (40 simulated residents, labelled).
3. **20–35 s:** a phone joins, picks B-102 and drops its parchi, which lands on the board.
4. **35–55 s:** close → reveal counter → draw → slots fill.
5. **55–70 s:** phone result P-04 → Verify → MATCHES stamp.
6. **70–80 s:** "Draw again", then REVERTED.
7. **80–90 s:** seed card; "Live on Monad testnet"; URL.
