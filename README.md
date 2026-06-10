# Telegram Bot

A Telegram bot with MongoDB-backed user approval, admin management, API integration, and deployment-ready process files for Railway, Heroku-compatible platforms, and Docker.

## Features

- 🔐 User approval system with expiration dates
- 👑 Admin commands for user management
- 🆔 `/id` command to show your Telegram user ID for deployment setup
- 📊 Usage statistics and logging
- 💾 MongoDB database for persistent storage
- 🔄 24/7 deployment ready with `Procfile`, `runtime.txt`, `railway.json`, and `Dockerfile`

## Prerequisites

- Python 3.11 or higher
- MongoDB database (Atlas or local)
- Telegram Bot Token from @BotFather
- External API endpoint with authentication key or URL template

## Environment Variables Setup

Copy `.env.example` to `.env` for local runs, or set the same variables in your hosting provider dashboard. Never commit real bot tokens, API keys, or MongoDB passwords.

```env
BOT_TOKEN=your_telegram_bot_token_here
MONGODB_URI=mongodb+srv://username:password@cluster.example.mongodb.net/?appName=Cluster0
DATABASE_NAME=attack_bot
ADMIN_IDS=123456789,987654321
```

### Admin IDs

If you only have one owner/admin user, set `ADMIN_IDS` to that Telegram user ID:

```env
ADMIN_IDS=123456789
```

You can also set either `OWNER_USER_ID` or `USER_ID` instead of `ADMIN_IDS`. The bot checks admin IDs in this order:

1. `ADMIN_IDS` (comma-separated list)
2. `OWNER_USER_ID`
3. `USER_ID`
4. Built-in fallback ID

To find your Telegram user ID after the bot is running, send `/id` to the bot.

### API Configuration

The bot supports two API modes.

#### JSON API mode

Use this mode when your API accepts JSON requests at `/api/v1/attack` and uses an `x-api-key` header:

```env
API_URL=https://your-api-domain.com
API_KEY=your_api_key_here
```

#### URL template API mode

Use this mode when your provider gives you one full URL containing placeholders. Keep the placeholders exactly as shown; the bot replaces them when `/attack ip port duration` is used.

```env
API_URL=TinyREF/1.0
API_KEY=https://example.com/APIv2?key=YOUR_KEY&target=TARGET&port=PORT&duration=TIME&method=METHOD
DEFAULT_ATTACK_METHOD=METHOD
```

In URL template mode:

- `TARGET` is replaced with the IP argument.
- `PORT` is replaced with the port argument.
- `TIME` is replaced with the duration argument.
- `METHOD` is replaced with `ATTACK_METHOD` if set, otherwise `DEFAULT_ATTACK_METHOD`.
- `API_URL` is sent as the HTTP `User-Agent` value for compatibility with providers that require a custom client name.

## Deployment

### Railway

1. Create a Railway project from this repository.
2. Add all required environment variables in Railway Variables.
3. Railway uses `railway.json` and starts the worker with `python bot.py`.

### Heroku-compatible platforms

The repository includes a root-level `Procfile` and `runtime.txt`:

```text
worker: python bot.py
```

Scale the worker process after deployment.

### Docker

Build and run with:

```bash
docker build -t telegram-bot .
docker run --env-file .env telegram-bot
```
