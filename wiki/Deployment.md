# Deployment

## Infrastructure

| Service | Platform | URL |
|---------|----------|-----|
| trustgram-ui | Cloudflare Pages | https://trustgram-ui.pages.dev |
| trustgram-bot | Render | https://trustgram-bot.onrender.com |
| trustgram-crypto | Cloudflare Pages | https://trustgram-crypto.pages.dev |

## Continuous Deployment

All three repositories are connected to their hosting platforms via GitHub integration.
Every push to `main` triggers an automatic deployment.

```
git push origin main → automatic deploy (30-60 seconds)
```

## Environment Variables (trustgram-bot)

| Variable | Description |
|----------|-------------|
| `TOKEN` | Telegram Bot Token (from @BotFather) |
| `WEBAPP_URL` | Frontend URL (https://trustgram-ui.pages.dev) |

Set in Render dashboard → Environment.

## Webhook Setup

After deploying trustgram-bot, register the Telegram webhook:

```
https://api.telegram.org/bot<TOKEN>/setWebhook?url=https://trustgram-bot.onrender.com/webhook
```

Verify:
```
https://api.telegram.org/bot<TOKEN>/getWebhookInfo
```

## Local Development

**Backend:**
```bash
pip install -r requirements.txt
TOKEN=... WEBAPP_URL=... uvicorn main:app --reload
```

**Frontend:**
Open `index.html` directly in browser or use any static server:
```bash
npx serve .
```
