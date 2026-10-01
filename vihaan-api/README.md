# VIHAAN Proposal API (prototype)

This Worker uses **Cloudflare Workers AI** through the `AI` binding. The browser sends business-context fields to `POST /api/proposal`; no model-provider API key is exposed to the browser, and a Gemini API key is not required for this configuration.

## Prerequisites

- Node.js and npm
- A Cloudflare account with Workers AI available
- Wrangler CLI

## Local development

1. From this directory, install Wrangler if needed and authenticate:

   ```bash
   npm install --global wrangler
   wrangler login
   ```

2. Create or update the local `.dev.vars` file with the access code used by your local VIHAAN preview:

   ```dotenv
   VIHAAN_ACCESS_CODE="your-local-access-code"
   ```

   Keep `.dev.vars` private and untracked. Never commit it or paste access codes into source files.

3. Confirm `wrangler.toml` includes the Workers AI binding:

   ```toml
   [ai]
   binding = "AI"
   ```

4. Start the Worker:

   ```bash
   npx wrangler dev
   ```

5. Open the local VIHAAN preview and submit a business scenario. The Worker uses `WORKERS_AI_MODEL` from `wrangler.toml`, currently `@cf/meta/llama-3.3-70b-instruct-fp8-fast`.

## Deployment

Configure the production access code as a Worker secret, then deploy only when explicitly approved:

```bash
npx wrangler secret put VIHAAN_ACCESS_CODE
npx wrangler deploy
```

The `[ai]` binding in `wrangler.toml` connects the Worker to Cloudflare Workers AI. Check Cloudflare's current model availability, account usage allowance, and pricing before relying on a model for ongoing use.

## Safety and behavior

- The proposal is an AI-generated draft, not verified client architecture.
- Generated nodes, runtime sequence, controls, and platform mapping are validated before being returned.
- The Worker enforces input size, field length, origin, access-code, and best-effort per-isolate request limits.
- The local Worker terminal may contain diagnostic error messages. Do not share credentials or sensitive business data when sharing logs.
