# VIHAAN Proposal API (prototype)

This Worker keeps the Google Gemini API key on the server side. The browser sends only the business-context fields to `POST /api/proposal`; it must never receive or store the Gemini key.

## Local prerequisites

- Node.js and npm
- A Cloudflare account and Wrangler CLI
- A Google Gemini API key from Google AI Studio (OpenAI is no longer required)

## Configure secrets

### 1. Obtain a Gemini API Key
1. Go to [Google AI Studio](https://aistudio.google.com/).
2. Sign in with your Google account.
3. Click **Get API key** and create a key in a new or existing project.

### 2. Configure for Deployed Environments
From this folder:

```bash
npm install --global wrangler
wrangler login
wrangler secret put GEMINI_API_KEY
wrangler secret put VIHAAN_ACCESS_CODE