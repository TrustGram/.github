# Architecture

## Components

| Component | Role | Stack |
|-----------|------|-------|
| trustgram-ui | Telegram Mini App, UI | Vanilla JS, Cloudflare Pages |
| trustgram-bot | API server, Telegram bot | Python, FastAPI, aiogram, Render |
| trustgram-crypto | Crypto engine | Vanilla JS, Web Crypto API, Cloudflare Pages |

## How They Interact

```
User
  ↓
Telegram (bot button)
  ↓
trustgram-ui (Cloudflare Pages)
  ↓ loads
trustgram-crypto (crypto.js via SRI)
  ↓ API requests
trustgram-bot (Render)
  ↓ notifications
Telegram Bot API
  ↓
Recipient
```

## Message Flow

```
Alice types a message
  ↓
crypto.js encrypts in browser (AES-256-GCM)
  ↓
Encrypted payload → trustgram-bot
  ↓
trustgram-bot puts payload in Bob's inbox
trustgram-bot sends Telegram notification to Bob
  ↓
Bob opens Mini App
  ↓
Browser fetches payload from server
  ↓
crypto.js decrypts locally
  ↓
Bob sees the message
```

## Security Principles

- Server never sees plaintext — only encrypted payloads
- Keys exist only in the user's browser
- Crypto code is isolated in trustgram-crypto and verified via SRI
- Telegram is used only as transport and address book
