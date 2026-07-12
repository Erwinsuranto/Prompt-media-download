
# Prompt-media-download
# 
```

You are a senior software architect and TypeScript engineer.

Build a production-ready Telegram Media Downloader Bot from scratch.

IMPORTANT

This is NOT only a YouTube downloader.

This project will become the download engine for a larger Telegram Drive ecosystem.

The project must be modular, scalable, production-ready and easy to extend.

==================================================

DO NOT

- Don't create demo code.
- Don't create placeholder code.
- Don't use TODO comments.
- Don't skip implementation.
- Don't generate fake APIs.
- Don't simplify architecture.

Everything must be production ready.

==================================================

MAIN GOAL

The bot must support downloading media from multiple platforms.

Current targets:

- YouTube
- Facebook
- Instagram
- X (Twitter)
- TikTok
- Threads
- Vimeo
- Reddit
- Pinterest
- SoundCloud

The architecture must allow adding new providers without modifying existing providers.

==================================================

TECH STACK

TypeScript

Node.js

grammY

yt-dlp

FFmpeg

dotenv

Pino Logger

==================================================

PROJECT STRUCTURE

Create a clean architecture.

Example:

bot/
commands/
handlers/
providers/
telegram/
storage/
cache/
core/
utils/
config/
logs/
types/

Do NOT create unnecessary folders.

==================================================

PROVIDER SYSTEM

Create provider architecture.

Each platform must be isolated.

Example

providers/

youtube/

facebook/

instagram/

twitter/

tiktok/

Each provider must expose the same interface.

==================================================

BOT FLOW

/start

Welcome message.

User sends any supported URL.

Bot automatically detects provider.

Fetch metadata.

Display

Thumbnail

Title

Duration

Uploader

File size if available.

Show Inline Keyboard.

Example

🎥 Video

🎵 Audio

❌ Cancel

If Video

Display available qualities.

Only show qualities actually available.

If Audio

Display available formats.

==================================================

DOWNLOAD

Download using yt-dlp.

Show progress.

Downloading...

Processing...

Uploading...

==================================================

TELEGRAM DRIVE INTEGRATION

Telegram Channel is the storage backend.

Do NOT store downloaded media permanently on VPS.

Workflow

Download

↓

Upload to Telegram Channel

↓

Save metadata

↓

Delete temporary local file

Store:

message_id

file_id

platform

original_url

title

duration

thumbnail

quality

mime_type

upload_date

==================================================

CACHE

Before downloading

Check whether the same URL already exists.

If already uploaded

Do NOT download again.

Simply CopyMessage from Telegram Channel.

==================================================

TEMP FILES

Store temporarily.

Automatically delete after upload.

==================================================

ERROR HANDLING

Handle

invalid URL

unsupported provider

private video

deleted media

age restriction

download failure

upload failure

timeout

network failure

==================================================

LOGGER

Create structured logging.

==================================================

SECURITY

Rate limit.

Flood protection.

Input validation.

==================================================

ADMIN

Admin command.

Statistics.

Users.

Uploads.

Errors.

==================================================

CONFIG

Use .env

Example

BOT_TOKEN=

CHANNEL_ID=

ADMIN_ID=

LOG_LEVEL=info

TMP_DIR=/tmp/media-downloader

==================================================

README

Generate complete documentation.

Installation.

Dependencies.

Ubuntu VPS setup.

Node installation.

FFmpeg installation.

yt-dlp installation.

Running bot.

Folder structure.

==================================================

IMPORTANT

This project is NOT a standalone downloader.

It is the future Media Downloader module of Telegram Drive.

Every architectural decision must make future integration with Telegram Drive easy.

Focus on building a clean foundation first.

Do NOT sacrifice architecture for speed.

Generate a production-ready project.



```
