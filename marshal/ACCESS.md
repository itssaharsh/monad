# ACCESS: what the build needs

| # | What | Why | Status (15:58 IST) |
|---|---|---|---|
| 1 | **Model key**: Gemini AI Studio (free), `LLM_API_KEY`; model `gemini-3.5-flash-lite` (~0.9 s, calls the pay tool with the loose prompt) | the agent | ✅ given; tested with one real call. It was pasted in chat, so **rotate it after the event** |
| 2 | **Server key** `0x9A2221e1951D76b0D9Aea2f31659C5f86F3D1A1A` (generated in the Claude session, never committed) with **~25 testnet MON**: 10 MON reserve + ~0.03 MON per chat × ~300 + 0.5 MON per owner phone | deployer + the agent's session key + owner-phone gas | ⏳ fund it from `0xB99C…d7B1`. Path B uses a fresh laptop key instead |
| 3 | **GitHub**: Claude GitHub App on `itssaharsh/dibs` | push the `marshal` branch | ⏳ pushes still refused |
| 4 | **Network** for the Claude session: Monad RPCs, monadvision, soliditylang binaries, Railway, objects.githubusercontent.com | deploy + Railway from here | ⏳ still blocked |
| 5 | **Railway**: account token from https://railway.com/account/tokens as `RAILWAY_API_TOKEN` | create service `marshal`, set variables, deploy | ⏳ |
| 6 | `PRESENTER_KEY` (random) | guards `/owner` and the handover | generated at deploy time |

Not needed: native MON for chatters (they never send transactions), a database, or any paid service.
