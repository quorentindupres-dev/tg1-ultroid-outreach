# TG1 — Ultroid (Telegram Multi-Account Userbot)

> **TG1** = Telegram #1 — The multi-account outreach foundation  
> ⭐ 2,962 stars | 🍴 8,039 forks | 🐍 Python + Telethon | 📅 Updated Apr 2026

## What TG1 Does

TG1 is a pluggable Telegram **userbot** — it runs as your real Telegram account (not a bot), giving you access to every action a human user can do. Combined with its **multi-account launcher** (up to 5 accounts simultaneously), it's the best open-source foundation for Telegram outreach.

## TG1 Multi-Account Architecture

```
TG1 (Ultroid) spawns 5 independent Python processes — 1 per account.
Each process has its own event loop, Redis DB, and broadcast list.
No shared state = no cross-contamination = safe isolation.

  multi_client.py
  ├── Process 1 → pyUltroid (SESSION)    → Account #1
  ├── Process 2 → pyUltroid (SESSION2)   → Account #2
  ├── Process 3 → pyUltroid (SESSION3)   → Account #3
  ├── Process 4 → pyUltroid (SESSION4)   → Account #4
  └── Process 5 → pyUltroid (SESSION5)   → Account #5
```

## TG1 Plugin Catalog (82 plugins)

### 🎯 OUTREACH (9 plugins)

| Plugin | File | Commands | What It Does |
|--------|------|----------|-------------|
| **globaltools** | `plugins/globaltools.py` | `.gcast`, `.gucast`, `.gadmincast`, `.gban`, `.ungban`, `.gmute`, `.ungmute`, `.gkick`, `.gpromote`, `.gdemote` | **The core outreach engine.** `.gcast` sends a message to EVERY group you're in. `.gucast` sends to EVERY PM conversation. `.gadmincast` only to groups where you're admin. Global ban/mute/kick/promote across all groups. |
| **broadcast** | `plugins/broadcast.py` | `.broadcast`, `.forward`, `.addch`, `.remch`, `.listchannels` | Reply-blast to a curated list of channels. Register channels with `.addch`, then `.broadcast` any reply to all of them. `.forward` does the same with polls/media. |
| **tag** | `plugins/tag.py` | `.tagall`, `.tagadmins`, `.tagowner`, `.tagbots`, `.tagon`, `.tagoff`, `.tagrec` | Tag members in a group. `.tagall` tags up to 99 non-bot members. `.tagadmins` tags admins only. `.tagon`/`.tagoff`/`.tagrec` for online/offline/recently-active. |
| **schedulemsg** | `plugins/schedulemsg.py` | `.schedule <msg> <delay>` | Schedule a message to send after N seconds/minutes/hours. Supports `1h`, `30m`, `100` (seconds). |
| **channelhacks** | `plugins/channelhacks.py` | `.shift <src>\|<dst>`, `.asource`, `.adest` | **Bulk copy** ALL messages between channels (2s delay). Auto-post relay: set source → destination and every new message auto-forwards. |
| **autoban** | `plugins/autoban.py` | `.autokick on/off` | DND mode — auto-kicks every new member who joins. |
| **forcesubscribe** | `plugins/forcesubscribe.py` | `.fsub <channel>` | Force users to join a channel before they can send messages in your group. |
| **pmpermit** | `plugins/pmpermit.py` | `.a`, `.approve`, `.da`, `.disapprove`, `.block` | PM gatekeeper — auto-responds to unknown senders, warns on spam, auto-blocks after threshold. |
| **fakeaction** | `plugins/fakeaction.py` | Fake typing, recording, gaming, video | Makes your account appear to be typing/recording/gaming in a chat. |

### 🛡️ GROUP MODERATION (10 plugins)

| Plugin | Commands | What It Does |
|--------|----------|-------------|
| **admintools** | Various admin commands | Admin utilities for group management |
| **antiflood** | Anti-flood config | Detects and blocks message flooding |
| **blacklist** | `.blacklist`, `.unblacklist` | Block specific users from interacting |
| **locks** | Lock/unlock chat features | Lock media, stickers, URLs, polls, etc. |
| **mute** | `.mute`, `.unmute` | Mute users in groups |
| **warn** | `.warn`, `.resetwarn` | Warn system with auto-action on threshold |
| **sudo** | Sudo mode | Delegate commands to trusted users |
| **profanityfilter** | Profanity detection | Auto-delete messages with bad words |
| **nsfwfilter** | NSFW detection | Block NSFW content |
| **_chatactions** | Auto-welcome, auto-goodbye, force-sub enforcement, gban enforcement, username logger | System plugin — handles automated group actions on join/leave. |

### 🤖 AI / CHATBOT (2 plugins)

| Plugin | What It Does |
|--------|-------------|
| **aiwrapper** | AI plugin framework — wrap any LLM for chat replies |
| **chatbot** | Auto-reply chatbot for configured users |

### 🎨 MEDIA (9 plugins)

| Plugin | What It Does |
|--------|-------------|
| **audiotools** | Audio effects and processing |
| **converter** | Format conversion |
| **giftools** | GIF manipulation |
| **glitch** | Glitch effects on images |
| **imagetools** | Image filters, effects, processing |
| **mediatools** | Media download/upload helpers |
| **stickertools** | Sticker pack management, sticker-to-image |
| **videotools** | Video processing, trimming |
| **vctools** | Voice chat management, music streaming |

### 📁 FILES & UTILITIES (10 plugins)

| Plugin | What It Does |
|--------|-------------|
| **fileshare** | File sharing utilities |
| **gdrive** | Google Drive integration |
| **pdftools** | PDF manipulation |
| **qrcode** | QR code generation |
| **downloadupload** | Download/upload manager |
| **webupload** | Upload files to web services |
| **ziptools** | Zip/unzip files |
| **compressor** | Media compression |
| **resize** | Image resizing |
| **fontgen** | Font/style generation |

### 🔧 TOOLS & MISC (33 plugins)

afk, beautify, bot (assistant), button, calculator, chats, cleanaction, core, database, devtools, echo, extra, filter, greetings, logo, misc, nightmode, notes, other, polls, profile, search, snips, specialtools, stories, twitter, unsplash, usage, utilities, variables, weather, words, writer, youtube, _help, _inline, _ultroid, _userlogs, _wspr, asstcmd

## TG1 Limitations

- **No volume spam** — no `.spam` command to flood messages
- **No reply-raid** — no auto-reply harassment
- **No cold-DM** — `.gucast` only hits existing PM dialogs, can't message new users
- **No member scraper** — can't export group member lists
- **No invite system** — no `.inviteall` to mass-add users to groups
- **No throttle** — `.gcast` sends sequentially with no delay (relies on FloodWait errors)

## TG1 Strengths Over TG2

- ✅ **Multi-account** (5 accounts, separate processes)
- ✅ **gucast** — blast ALL PMs (TG2 doesn't have this)
- ✅ **tagall** — single-message all-mention (TG2 uses bulk-5 or per-user)
- ✅ **schedule** — proper delayed message delivery
- ✅ **gban/gkick/gpromote** — global moderation across all groups
- ✅ **shift** — bulk copy all messages between channels
- ✅ **82 plugins** vs TG2's 56
- ✅ **2,962 stars** — 15x more battle-tested than TG2
- ✅ **8,039 forks** — massive community, lots of reference code
