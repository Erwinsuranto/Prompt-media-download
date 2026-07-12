
# Prompt-media-download





# Prompt 2 — Telegram Storage Engine
```


Continue the existing project.

Do NOT rewrite the architecture.

Build the next production-ready module.

========================================

MODULE

Telegram Storage Engine

========================================

This project will become the media engine of my Telegram Drive ecosystem.

Telegram Channel is the permanent storage backend.

Every downloaded media from every provider must be uploaded into Telegram.

Do NOT permanently store media on VPS.

========================================

WORKFLOW

User sends URL

↓

Provider downloads media

↓

Upload media to Telegram Channel

↓

Save metadata

↓

Delete temporary file

↓

Return file to user

========================================

The Telegram Storage Engine must become a reusable service.

Nothing inside providers should directly call Telegram APIs.

Providers should only return downloaded media.

Storage Engine handles everything related to Telegram.

========================================

Implement

TelegramStorage

UploadManager

MessageManager

FileCache

ThumbnailUploader

MediaSender

========================================

Store

message_id

file_id

chat_id

provider

original_url

title

description

duration

thumbnail

mime_type

quality

resolution

fps

bitrate

codec

size

upload_date

checksum

========================================

CACHE

Before downloading

Search database

Search checksum

Search original URL

Search normalized URL

If file already exists

Never download again.

Reuse Telegram CopyMessage.

========================================

MEDIA TYPES

Handle correctly

Video

Audio

Photo

Animation

Voice

Document

Sticker

Future media types.

========================================

UPLOAD

Automatically choose correct Telegram upload method.

Never upload everything as document.

Use

sendVideo

sendAudio

sendPhoto

sendDocument

etc.

========================================

LARGE FILES

Handle Telegram limits.

Support chunking if needed in future.

Prepare architecture.

========================================

THUMBNAILS

Upload thumbnails.

Reuse thumbnails.

Cache thumbnails.

========================================

DATABASE

Storage layer must be abstract.

No provider should know database implementation.

========================================

ERROR HANDLING

Upload timeout

FloodWait

Retry

Network failure

Expired file

Invalid file_id

========================================

LOGGING

Log every upload.

Log cache hits.

Log failures.

Log retries.

========================================

API

Expose clean methods

upload()

exists()

copy()

get()

deleteTemp()

saveMetadata()

========================================

README

Update architecture documentation.

Document Telegram Storage Engine.

Explain future Telegram Drive integration.

========================================

IMPORTANT

This module must become the heart of Telegram Drive.

Future providers like

YouTube

Facebook

Instagram

TikTok

Threads

Pinterest

Reddit

Vimeo

SoundCloud

must use this Storage Engine without changing its implementation.

Refactor existing code if necessary.

Push every completed change to GitHub.

Do not stop until the Telegram Storage Engine is production-ready.


```
# 
```


Do NOT stop at the current foundation.

Continue transforming this repository into a true production-ready Telegram Media Downloader Engine.

IMPORTANT

This project is NOT just a Telegram downloader.

This project will become the Media Download Engine for my future Telegram Drive ecosystem.

Everything you build must be designed for long-term scalability.

====================================

DO NOT

- use demo code
- use placeholder code
- leave TODO
- skip implementation
- simplify architecture
- break existing code

Everything must be production-ready.

====================================

HARDEN THE CURRENT PROJECT

Replace every weak implementation with production-grade code.

Implement:

- robust error handling
- retry mechanism
- download queue
- concurrency control
- graceful shutdown
- structured logging
- rate limiting
- input validation
- provider timeout
- upload retry
- download retry

====================================

PROVIDER SYSTEM

Create a modular provider architecture.

Providers must be completely independent.

Supported providers:

YouTube

Facebook

Instagram

TikTok

X (Twitter)

Threads

Reddit

Pinterest

Vimeo

SoundCloud

Future providers must only require creating a new provider folder.

No existing provider should require modification.

====================================

DOWNLOAD ENGINE

Detect provider automatically.

Fetch metadata.

Show

Title

Thumbnail

Duration

Available qualities

Available audio formats

File size

Uploader

Only show formats that actually exist.

====================================

TELEGRAM STORAGE

Telegram Channel is the permanent storage backend.

Never keep downloaded files permanently on VPS.

Workflow:

Download

↓

Upload to Telegram Channel

↓

Save metadata

↓

Delete temporary file

Store:

message_id

file_id

provider

original_url

title

duration

thumbnail

quality

mime_type

upload_date

checksum

====================================

CACHE

Before downloading

Search cache.

If media already exists

Do NOT download again.

Reuse Telegram message using CopyMessage.

====================================

DATABASE

Do NOT use JSON.

Use a production-ready database.

The storage layer must be abstract so I can switch databases later without changing business logic.

====================================

PROJECT STRUCTURE

Separate:

Bot

Providers

Telegram

Storage

Downloader

Cache

Queue

Config

Core

Utils

Types

Logger

====================================

ADMIN

Implement:

Statistics

Active downloads

Queue status

Provider status

Errors

Logs

====================================

README

Rewrite README completely.

Include:

Ubuntu VPS installation

Node installation

FFmpeg

yt-dlp

Environment variables

Running bot

Project architecture

Folder explanation

Troubleshooting

====================================

VERY IMPORTANT

Every architectural decision must prepare this project for future integration with Telegram Drive.

The downloader must become a reusable module.

In the future Telegram Drive will call this engine instead of implementing download logic itself.

Do not stop until the project is production-ready by enterprise standards.

If you need to refactor the existing code to achieve this, do it.


```
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
