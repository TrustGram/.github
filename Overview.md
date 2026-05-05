# TrustGram

TrustGram is an end-to-end encrypted messenger built on top of Telegram. It uses Telegram as a delivery transport and address book, while all encryption happens entirely in the user's browser.

## Key Principles

- **Zero-trust to Telegram** — Telegram only sees encrypted payloads, never plaintext
- **Keys never leave the device** — private keys are stored in the browser only
- **Transparent cryptography** — crypto code is isolated, auditable, and verifiable via SRI hash
- **Minimal user effort** — encryption is automatic, no manual key management required

## Repositories

| Repository | Description | Hosting |
|------------|-------------|---------|
| [trustgram-ui](https://github.com/TrustGram/trustgram-ui) | Telegram Mini App frontend | Cloudflare Pages |
| [trustgram-bot](https://github.com/TrustGram/trustgram-bot) | Bot + API server | Render |
| [trustgram-crypto](https://github.com/TrustGram/trustgram-crypto) | Cryptographic engine | Cloudflare Pages |
