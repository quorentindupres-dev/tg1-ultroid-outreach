# Deploy TG1 (Ultroid) on Windows Docker

## Prerequisites

- Docker Desktop for Windows installed
- Telegram API credentials from https://my.telegram.org/apps
- A Telegram account to use as the bot

## Step 1: Get API Credentials

1. Go to https://my.telegram.org/apps
2. Log in with your Telegram account
3. Create an app → get `api_id` and `api_hash`
4. Get a bot token from @BotFather on Telegram (optional, for assistant bot)

## Step 2: Set up environment

Create `.env` file in the project root:

```env
# Required
API_ID=12345678
API_HASH=abcdef1234567890abcdef1234567890
REDIS_URI=redis://redis:6379
REDIS_PASSWORD=

# Session string (generate this first — see below)
SESSION=your_session_string_here

# Optional multi-account (up to 5)
# SESSION2=your_second_session
# SESSION3=your_third_session

# Optional
BOT_TOKEN=123456:ABC-DEF1234ghikl
LOG_CHANNEL=-1001234567890
```

## Step 3: Generate Session String

```powershell
# In the project directory:
docker compose run --rm worker python3 sessiongen
```

Follow the prompts — enter phone number, verification code, 2FA password if set. You'll get a session string. Paste it into `.env` as `SESSION`.

## Step 4: Start

```powershell
docker compose up -d
```

## Step 5: Interact via Telegram

Open Telegram, go to "Saved Messages" (or any chat with the bot). Send:

```
/ping              — Check if bot is alive
/stats             — Account statistics
/gcast Hello       — Broadcast "Hello" to ALL your groups
/gucast Hi there   — Broadcast to ALL PM conversations
/tagall            — Tag everyone in current group
/help              — Full command list
```

## Step 6: Check logs

```powershell
docker compose logs -f worker
```

## Multi-Account Setup

Add sessions in `.env`:
```env
SESSION=account1_session_string
SESSION2=account2_session_string
SESSION3=account3_session_string
SESSION4=account4_session_string
SESSION5=account5_session_string
```

Each account runs as a separate process with its own Redis DB, broadcast list, and settings. No cross-contamination.

## Health Check

```powershell
docker compose ps
```

All accounts show as `healthy` when connected.

## Troubleshooting

| Issue | Fix |
|-------|-----|
| "SESSION not found" | Re-run `docker compose run --rm worker python3 sessiongen` |
| FloodWait error | Wait the specified seconds, reduce send frequency |
| Account banned | Use aged accounts, add delays between sends |
| Docker not starting | Check `docker compose logs` for Python import errors |
