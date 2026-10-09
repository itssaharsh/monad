---
name: Parchi
scope: sprint
one_line: "Parchi runs society draws nobody can rig."
ambition: L2  # consumer, live-room surface; the motion explains real events (entries landing, slots allocated); task flows on the phone stay L1
registers: { landing: expressive, board: expressive, phone: productive, result: celebratory, rerun-attempt: serious, verify: productive }
direction: "derived from a housing-society AGM: notice-board paper canvas · carbon ink · rubber-stamp violet accent · parking-stencil yellow for slots · display Big Shoulders Stencil (candidates: Big Shoulders Stencil, Saira Stencil One, Stardos Stencil)"
personality: mechanical
dials: { variance: 5, motion: 6, density: 4 }
stack: { app: "Next 16 App Router — needs API routes (relayer) + dynamic routes", css: "Tailwind 4 with :root tokens — fast to build", chain: "viem 2 — burner keys, EIP-712, reads; no wagmi (no wallet extension anywhere)", qr: "qrcode.react — QR on the board", motion: "CSS + WAAPI only; no motion library", fonts: "Google Fonts via next/font" }
archetype: live-room board + phone companion
viewports: [390x844, 1440x900, 1920x1080]
signature: { interaction: "each entry drops a folded parchi into the bowl, stamped with its block number", visual: "parking-stencil slot labels (P-01) on concrete tiles, filled by unfolding parchis" }
wow: "host taps Draw → board acknowledges in <100 ms → parchis fly from the bowl onto stencilled slots and unfold to flat numbers (≈1.2 s) → slots stay filled, waitlist ranks appear → seed card + tx link; every phone's Verify recomputes and stamps MATCHES"
demo: { seed: "Blitz Heights CHS, 24 flats A-101…D-106, 6 slots", flag: "?demo=1", state_param: "?state=", reset: "alt+shift+r", replay: "alt+shift+p", guest: true }
live_vs_simulated: [ "draw contract, entries, reveals, allocation: live on Monad testnet", "'Add 20 residents' rehearsal button: real txs from server-held keys, labelled Simulated residents" ]
deviations: [ "no wallet connect UI at all (B15 assumes one): burner keys + relayer remove it, which is the point for an AGM" ]
---

## 0. Brief + context profile
- **One line:** Parchi runs society draws nobody can rig.
- **User and moment:** resident in an AGM hall, phone in hand; committee member at a laptop wired to a projector. Once or twice a year.
- **Primary action:** drop (your parchi into the bowl).
- **Hero object:** the bowl of folded parchis → the stencilled parking slots.
- **Product moment:** the allocation lands on the slots and on your phone.
- **Demo moment:** 60 phones drop parchis; the bowl fills block by block; one tap and the slots fill; everyone's phone says MATCHES.
- **Differentiator:** the list is frozen before randomness exists, randomness comes from the room, and any phone recomputes the result.
- **Artifact:** your slot (or waitlist rank) with a verifiable receipt: draw id, seed, tx.
- **World inventory:** folded paper chits (parchi) · steel bowl · notice board with pinned circular · violet rubber stamp ("SECRETARY", "VERIFIED") · yellow stencil paint on grey stilt-parking concrete.
- **Moving truth:** block numbers and entries per block, read from the chain.
- **Judging:** 3-minute live demo to builders who vote on phones (judging-intel R8–R10); projector + their phones.
- **Budget:** ~2.5 h of UI inside a ~6 h build.

| Factor | Pole | Effect |
|---|---|---|
| Task frequency | once | guidance, one expressive moment |
| Data density | ≤150 entries | big type; grid of chits virtualised past 120 |
| Stakes | shared assets, neighbours | serious tone on the re-run attempt; no confetti on losing |
| Audience | general public (residents) | plain words: "parchi", "slot", "waitlist"; no "commit/reveal" on the phone |
| Use scene | projector + phones in a hall | light paper theme (projectors wash out dark greys); 56 px phone targets |
| Emotional target | *witnessed* | everyone sees the same thing happen at once |
| Personality | stencil, stamp, paper | mechanical: snaps, thunks, no wobble |

## 1. Judge tests
- **5 s (board):** big title "Parking draw · Blitz Heights CHS", the bowl with N parchis, QR "Scan to drop your parchi". Stranger answers: "a live draw for parking slots the room joins".
- **30 s:** a phone joins, its parchi lands on the board with a block number within ~1 s.
- **Demo-critical screens:** S3 Board, S4 Phone. **Stills:** (1) bowl filling with the block ticker, (2) slots filled + seed card, (3) phone with the VERIFIED stamp.

## 2. Demo script (3:00)
0:00 board already open, QR showing: "Every Pune society has had this fight — the parking draw." · 0:10 "Chits in a bowl, the secretary's kid picks, half the building says it was rigged. Scan this — you're all residents of Blitz Heights now." · 0:20–1:00 room drops parchis; narrate the block ticker ("that's 40 entries in 3 blocks") · 1:00 Close entries → "list frozen: this hash is on-chain, nobody can be added or left out" · 1:10 phones reveal automatically (counter) · 1:20 Draw → slots fill · 1:35 "Tap Verify — your phone just recomputed the draw from the chain." · 1:50 host taps "Draw again" → red REVERTED: already drawn · 2:05 seed card: why the last person can't bias it · 2:30 scope limit + what's next · 2:50 one line + URL.

## 3. Screen inventory
| id | route | why | entered from | primary action | states |
|---|---|---|---|---|---|
| S1 | `/` | explain + start | link | Start a draw | — |
| S2 | `/new` | create the draw (pre-filled) | S1 | Freeze list & open entries | idle · submitting · error |
| S3 | `/d/[id]` | the projector board | S2 | Close entries → Draw | open · frozen · revealing · drawn · rerun-reverted · error |
| S4 | `/j/[id]` | resident's phone | QR | Drop my parchi | pick-flat · dropping · in-bowl · revealing · result-won · result-waitlist · closed-late · error |
| S5 | `/v/[id]` | recompute in browser | S4/S3 | Verify | computing · matches · mismatch · error |

## 4. Flow map
S2 --create--> S3(open) ; S4 --drop--> S4(in-bowl) ; S3 --close--> S3(frozen→revealing) ; S4 auto-reveal ; S3 --draw--> S3(drawn) ; S4(result) --Verify--> S5 ; S3 --Draw again--> S3(rerun-reverted)

## 5. Screens

### S3 Board (1440×900 and 1920×1080; 12-col grid, 32 px gutter)
Blueprint: top bar 64 px (wordmark · draw title · "Monad testnet" pill · phase stepper) / main: left 7 cols **the bowl stage**, right 5 cols **QR card** (open) → **slot grid** (drawn) / bottom strip 56 px **block ticker**.

| Element | Tier | Register | Levers (grayscale first) | States | Transition |
|---|---|---|---|---|---|
| Bowl stage: steel bowl (CSS radial gradient, 420 px) with folded parchis heaped inside, count "47 parchis" in stencil 120 px under it | primary | expressive | largest region; only element with depth (inner shadow); count is the biggest type on screen | open: chits drop in · frozen: violet "LIST FROZEN" stamp + list hash under the count · revealing: chits flip one by one as reveals land, "31/47 revealed" · error: last good state stays, "Chain is slow, retrying" line | each new entry: chit falls 180 px into the bowl, 420 ms ease-in, lands with a 2 px settle; label = flat + "#block" |
| QR card | secondary | productive | white raised surface, 320 px QR, "Scan to drop your parchi", join URL in mono | frozen: QR greys out, "Entries closed" | fades through to slot grid at Draw |
| Slot grid (after draw) | primary (replaces QR card) | celebratory | concrete tiles 2×3, yellow stencil "P-01"…, each filled by an unfolded parchi with the flat number 40 px | rerun-reverted: whole grid stays, red banner above | parchis fly bowl→tile in slot order, 90 ms stagger, unfold 240 ms |
| Phase stepper | tertiary | system | 4 steps, 13 px caps, current step ink, others muted | — | step underline slides 200 ms |
| Host controls | interactive | serious | bottom-right, one filled button per phase: "Close entries" → "Draw slots"; a quiet text button "Draw again" only after drawn | blocked with reason ("Waiting for reveals: 31/47 — draw anyway after 20 s") | label morphs |
| Block ticker | secondary | system | mono 15 px strip: "Block 12,401,233 · +6 entries · 0.4 s" items scroll left; pause control "Pause ticker" | empty: "Waiting for the first parchi" | items enter from right, 160 ms |
| Seed card (drawn) | tertiary → click for depth | productive | mono card: list hash, reveals used, block hash after close, seed, tx ↗ | — | slides up 240 ms after slots fill |
| Rerun banner | alert | serious | full-width red-ink banner: "Reverted on-chain: this draw already happened. A draw runs once." + tx ↗ | — | shake-free; 200 ms fade |

### S4 Phone (390×844)
Top: draw title + society 15 px; body one card; sticky bottom primary button 56 px.

| Element | Tier | Levers | States |
|---|---|---|---|
| Flat picker: grid of flat chips (A-101…), claimed ones struck through | primary (pick-flat) | 4-col chips 64 px, mono | claimed · mine · disabled-after-close |
| "Drop my parchi" | interactive | full-width violet, sticky bottom | dropping: "Dropping… block 12,401,233" · in-bowl: becomes a folded parchi illustration with "In the bowl · #block" |
| Status card | secondary | big paper chit with your flat | in-bowl: "Wait for the draw. Keep this page open." · revealing: "Adding your randomness…" · result-won: stencil "P-03" 96 px on concrete tile · result-waitlist: "Waitlist #4" · closed-late: "Entries closed before you joined" |
| Verify link | interactive | text button under the result → S5 | — |

### S5 Verify (phone or desktop)
Checklist computed in the browser, each line ticks as it completes: (1) eligible list matches frozen hash, (2) N reveals match their commits, (3) block hash after close read, (4) seed recomputed, (5) shuffle recomputed → same slots. Ends on a violet rubber stamp "MATCHES" (rotated −6°, thunk scale 1.3→1, 180 ms). Mismatch: red "DOES NOT MATCH" with the first differing slot.

## 9. Copy deck (key)
- Landing H1: "Society draws nobody can rig." Sub: "Freeze the list. Let the whole room add randomness. Anyone can check the result."
- Buttons: "Start a draw" · "Freeze list & open entries" · "Drop my parchi" · "Close entries" · "Draw slots" · "Draw again" · "Verify on my phone"
- Labels: "parchis in the bowl" · "List frozen" · "Waitlist #4" · "Monad testnet" · "Simulated residents" (rehearsal only)
- Errors: "Couldn't reach the chain. Your parchi is saved on this phone; we'll retry." · "That flat already dropped a parchi."

## 10. Brand
Wordmark "parchi" lowercase in Bricolage Grotesque 700 with the dot of the i as a small folded chit. Favicon: violet folded chit. OG: bowl + "Society draws nobody can rig."

## 11. Real-product checks
Value: yes (the allocation is the deliverable) · Clarity: phone has one action per state · Trust: list hash + seed + verify page · Feedback: every tap acknowledges <100 ms, then a block number · Failure: entries cached on phone and retried · 10×: 1,000 flats → board virtualises chits beyond 120 · Maintainability: one contract, one route, five pages.

## 12. Don'ts
No neon-on-black, no gradients on surfaces, no "WAGMI/gm", no confetti for waitlisted residents, no wallet-connect button, no fake activity: every chit is a real entry.

## 13. Acceptance (Sprint gates 1–4)
Cut pass · B11 anti-slop grep · axe + keyboard + 320 px · 5 s test on the three stills · golden path ≤3 clicks · `?state=` reaches every S3/S4 state · reset (alt+shift+r) works 3× in a row.

## 14. Tokens
```css
:root{
  --canvas:#F3EFE6;      /* notice-board paper */
  --surface-1:#FBF9F4;   /* chit paper */
  --surface-sunken:#E6E1D6;
  --concrete:#8E8C86; --concrete-dark:#6F6D68;
  --ink:#1C1B19; --ink-muted:#5E5A52; --ink-faint:#9A958A;
  --accent:#5B3FD6;      /* rubber-stamp violet */
  --accent-ink:#FFFFFF;
  --stencil:#F2C230;     /* parking paint */
  --steel-1:#D9DCDF; --steel-2:#9FA4AA; --steel-3:#5C6168;
  --danger:#B3261E; --success:#1F7A4D;
  --r-sm:4px; --r-md:8px; --r-chit:2px;
  --ff-display:"Big Shoulders Stencil Display",system-ui; --ff-body:"Bricolage Grotesque",system-ui; --ff-mono:"JetBrains Mono",ui-monospace;
  --ease-drop:cubic-bezier(.55,0,1,.45); --ease-out:cubic-bezier(.2,.8,.2,1);
}
```
