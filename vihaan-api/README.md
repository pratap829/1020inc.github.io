# VIHAAN Proposal API (prototype)

This Worker keeps the OpenAI API key on the server side. The browser sends only the business-context fields to `POST /api/proposal`; it must never receive or store the OpenAI key.

## Local prerequisites

- Node.js and npm
- A Cloudflare account and Wrangler CLI
- An OpenAI API key with API billing configured

## Configure secrets

From this folder:

```bash
npm install --global wrangler
wrangler login
wrangler secret put OPENAI_API_KEY
wrangler secret put VIHAAN_ACCESS_CODE
```

Paste the key only into the Wrangler secret prompt. Do not place it in source files, frontend config, chat messages, screenshots, or GitHub.

For local-only testing, create a local `.dev.vars` file containing:

```
OPENAI_API_KEY=your-key-here
VIHAAN_ACCESS_CODE=replace-with-a-long-random-prototype-access-code
OPENAI_MODEL=gpt-4.1-mini
ALLOWED_ORIGINS=http://127.0.0.1:5500,http://localhost:5500
```

Never commit `.dev.vars`. Use the secret command for deployed environments.

## Run locally

```bash
wrangler dev --port 8787
```

The endpoint will be `http://127.0.0.1:8787/api/proposal`. Set the frontend's `window.VIHAAN_CONFIG.apiUrl` to that URL in `vihaan/config.js` for local testing.

## Deploy

After testing locally:

```bash
wrangler deploy
```

Configure `OPENAI_API_KEY` as a Worker secret before use. Update `ALLOWED_ORIGINS` to the exact production origin(s) that serve VIHAAN. Keep localhost origins only while local development is needed.

## Security notes

- The OpenAI key and shared prototype access code are read only from Worker secrets. The browser prompts for the prototype access code and keeps it in session storage for the current tab session; this is not the OpenAI API key.
- Request body and field lengths are capped; the output schema is checked; provider errors are not exposed to the browser; responses are not cached.
- A server-side shared access code gates model calls for private prototype testing; rotate it if exposed. This is not a full user identity system. The in-memory limiter is best-effort per Worker isolate, not a production-grade global limit. Before public exposure, configure Cloudflare edge rate limiting and a stronger access-control mechanism (for example Cloudflare Access or authenticated sessions). CORS is not authentication.
- Do not send customer PII, secrets, or live production records in the prototype.
- This is a prototype integration, not a production security certification. Review privacy, logging, budget alerts, usage limits, and access controls before external users can reach the endpoint.
