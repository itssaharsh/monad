# Marshal: what's left and how to finish it

**State (16:18 IST): LIVE, deployed via path A from the Claude session.** Don't run path B.
- App: https://marshal-production-7481.up.railway.app (Railway project `dibs`, service `marshal`; Dibs `web` untouched)
- MarshalWallet: `0x16b33447c69899aD7606a78e6b302DE54E551949` · tINR `0x7D1b24eb8B02f27c873C08ca25dAd9Ab4f856d70` (Monad testnet)
- Owner: `0xB4763923367280099a689071b84a4C8Ad095Cb3b` (pre-funded 0.3 MON from the Dibs operator; handover done)
- Server/agent: `0x9A2221e1951D76b0D9Aea2f31659C5f86F3D1A1A` (6 MON from the Dibs operator; ~5.3 left, ~170 chats)
- Live smoke: attack → REFUSED NotAllowlisted (tx 0x84e7cb67…92c6)
- Don't redeploy during the pitch: recorded conversations live in the container and are lost on redeploy.

Below is the original plan, kept for the record.

## Path A: the Claude session finishes it (needs all three)
1. Claude GitHub App installed on `itssaharsh/dibs`.
2. Network: Full, or the allowed domains listed in the chat.
3. `RAILWAY_API_TOKEN` (and optionally `LLM_API_KEY`) in the environment variables.

Then Claude will: push `marshal` → deploy contracts with the server key `0x9A22…1A1A` → create Railway service `marshal` → set variables → `railway up` → smoke-test the live URL.

## Path B: paste this into your laptop coding agent (it has network + Railway CLI)
```
In my Dibs repo (github.com/itssaharsh/dibs, the monad-blitz-pune fork), apply the finished Marshal build and deploy it. Do exactly this; stop and report on any failure.

1. git fetch origin && git checkout -b marshal origin/main   # main must be at fa4396a "state: deployed on Railway"
   curl -sL https://raw.githubusercontent.com/itssaharsh/monad/fix/nice-pasteur-fkd58q/marshal/marshal.patch -o /tmp/marshal.patch
   git am --3way /tmp/marshal.patch          # creates 5 commits authored "Saharsh"
   npm ci && (cd contracts && forge test && forge build) && npm test && npm run typecheck
2. Server key: a FRESH key, not the Dibs operator (both services run at once; one key per process).
   cast wallet new  → send it 25 testnet MON from 0xB99CE2Cc7E3fDaf405fa4f807642B39A3590d7B1.
   Put it in .env as SERVER_PK. Also set PRESENTER_KEY=$(openssl rand -hex 16) and LLM_API_KEY=<Gemini key> in .env.
3. Deploy: set -a; . ./.env; set +a; node scripts/deploy.mjs && node scripts/check-deploy.mjs
   (writes deployments/testnet.json: wallet with 10,000 tINR, 3 shops allowlisted, limits 3000/8000/1000)
   git add deployments/testnet.json && git commit -m "T02: MarshalWallet deployed on Monad testnet"
4. Railway (same project as Dibs, NEW service; do NOT touch the Dibs 'web' service):
   railway add --service marshal   (check `railway add --help` for this CLI version)
   railway variables --service marshal --set SERVER_PK=... --set PRESENTER_KEY=... --set LLM_API_KEY=... \
     --set LLM_MODEL=gemini-3.5-flash-lite --set MONAD_RPC_HTTP=https://testnet-rpc.monad.xyz,https://rpc.ankr.com/monad_testnet,https://rpc-testnet.monadinfra.com
   railway up --service marshal --ci && railway domain --service marshal
5. Smoke: curl <url>/api/health  (ok:true, server.mon > 12)
   node scripts/chat-smoke.mjs <url>                                   → refused NotAllowlisted
   node scripts/chat-smoke.mjs <url> "Buy me a notebook from Pune Books for 250 rupees"   → allowed
6. Put the live URL and the MarshalWallet address into README.md (replace LIVE_URL and MARSHAL_ADDRESS), commit, git push -u origin marshal.
Reply with: the live URL, the MarshalWallet address, the deploy tx hashes, and the smoke output.
```

## Then you (two minutes)
- Phone: open `<url>/owner?key=<PRESENTER_KEY>` → **Take ownership** (the server hands the wallet to your phone).
- Projector: `<url>/board`.
- **Backup recording now:** chat one attack from a second phone and screen-record the board for 60–90 s.

## Decision gate at 17:05 IST: Marshal or Dibs?
Submit **Marshal** if the live URL passes this once: attack REFUSED → ₹250 ALLOWED → ₹2,500 WAITING → Approve on the owner phone → Revoke → next attempt REFUSED · Agent revoked. Otherwise submit **Dibs** (already live and recorded).

If Marshal:
- GitHub → itssaharsh/dibs → Settings: rename the repo to `marshal`, set the default branch to `marshal`, and set the description to "Marshal: the wallet that says no when your AI agent says yes".
- The Railway Dibs service can stay up; it costs nothing in the pitch.

## Scope ladder (cut from the top if anything slips)
1. Phone tINR balance in the header.
2. The replay hash check (keep the replay itself).
3. Owner key on the phone → fall back to the server acting as owner. Skip "Take ownership"; you'd need a `/api/admin/*` route. Ask Claude.
4. Live model → scripted stand-in only. Unset LLM_API_KEY; the strips are labelled "scripted".
- **Never cut:** a refusal recorded on-chain without a revert, the red REFUSED strip on the board, and the balance holding.
