
# Prompt-media-download






# 
```


Repository is already up to date.

Current status:

- npm test: PASS (39/39)
- Only npm run build fails.
- Remaining build errors: 8 errors in only 3 files.

Do NOT stop until `npm run build` succeeds.

Requirements:
- Read the repository and modify the code directly.
- Commit after every logical fix.
- Push every commit to main.
- After every push run the build again.
- Continue automatically until build is green.
- Do not ask me for confirmation.
- Do not use @ts-ignore.
- Do not disable TypeScript.
- Do not weaken typings.
- Preserve runtime behaviour.

Current errors to fix:

1. src/core/ProcessRunner.ts
- Cannot find module '../logs/logger'

2. src/core/YtDlpClient.ts
- config.ytDlpPath -> should use the current configuration API.
- config.downloadTimeoutMs -> should use the current configuration API.

3. src/telegram/MediaSender.ts
- Fix thumbnail type errors using the correct grammY InputFile API.
- Do not cast to any.

Goal:
Leave the repository production-ready with:

npm test
PASS

npm run build
PASS

Then push the final commit to main and print:

✔ Build passed
✔ Tests passed
✔ Repository is production ready


```

# 
```

You started this refactor and you are now responsible for finishing it.

This repository has already passed all tests:

- npm test → 39/39 PASS

The only remaining problem is:

- npm run build → TypeScript build errors

Your task is to continue your own work until the repository builds successfully.

Requirements:

1. Read the current repository source code (not commit metadata only).
2. Compare your latest commits with the current main branch.
3. Identify every incomplete refactor introduced during your previous changes.
4. Run the build.
5. Fix the TypeScript errors in small logical batches.
6. After each successful batch:
   - run npm run build
   - run npm test
   - commit
   - push to main
7. Repeat until:
   - npm run build succeeds with zero TypeScript errors
   - npm test still passes 100%
   - GitHub Actions are green

Rules:

- Do NOT rewrite the project.
- Do NOT revert completed work.
- Do NOT delete files unless they are confirmed obsolete.
- Do NOT use any.
- Do NOT use @ts-ignore.
- Do NOT disable TypeScript.
- Preserve all existing behaviour.
- Fix only the real root causes.

If you have access to the repository contents, continue working autonomously without asking me after every commit.

If you encounter a genuine technical limitation, stop only after providing:
- the exact limitation,
- the exact file(s) affected,
- the exact line(s),
- the exact evidence,
- and why that limitation prevents further progress.

Do not stop simply because there are many errors. Continue until the build is green or until you reach a verified technical blocker that cannot be bypassed.

The repository should be left in a production-ready state.



```

# 
```


You are taking over this repository from another AI agent (Claude Opus 4.8).

Do not modify any code yet.

Your first task is to perform a COMPLETE repository audit.

Repository:
https://github.com/zenolambetuna/telegram-media-downloader

Audit requirements:

1. Read the entire repository source code.
2. Read the latest commit history.
3. Compare the latest commits to understand what Claude changed.
4. Identify all architectural changes.
5. Identify every unfinished refactor.
6. Detect broken interfaces, renamed functions, deleted files, missing exports, incompatible types, and outdated imports.
7. Determine why:
   - npm test passes
   - but npm run build still fails.
8. Identify the true root cause instead of guessing.

Do NOT modify any files during this audit.

Produce a report containing:
- Repository architecture overview.
- Files modified in the recent commits.
- Incomplete refactors.
- Broken dependencies between modules.
- Build blockers grouped by root cause.
- Recommended repair order from highest to lowest priority.

Only after the audit is complete, wait for my approval before making any code changes.


```


# 
```


You already have access to my GitHub repository through GitHub MCP.

Do not stop after one response.

Continue working until every TypeScript build error is fixed or until you reach a real technical limitation.

Workflow:

1. Read the repository source code.
2. Sync with the latest main branch.
3. Run the build.
4. Fix the TypeScript errors in logical batches.
5. Run the build again.
6. Run the tests.
7. Commit and push only when a batch is successfully fixed.
8. Continue automatically with the next batch.

Repeat this process until:
- npm run build succeeds with zero TypeScript errors.
- npm test passes.
- The repository builds successfully.

Do not ask me to copy code unless you truly cannot access the repository.
Do not stop after fixing only one issue.
If you encounter a real limitation, explain exactly what prevented you from continuing and what evidence you need.

Otherwise, continue working autonomously until the build is green.


```

# 
```

You have full access to this GitHub repository.

DO NOT guess.
DO NOT ask me to paste build logs.
DO NOT ask me to copy file contents.

Your first task is to READ the ACTUAL repository source files, not only commit hashes or metadata.

Repository:
https://github.com/zenolambetuna/telegram-media-downloader

Requirements:

1. Read the repository source code completely.
2. Sync with the latest main branch.
3. Run:
   git pull origin main
   npm install
   npm run build
   npm test
4. Reproduce every TypeScript error locally.
5. Fix the errors in small logical batches.
6. After each batch:
   - run npm run build
   - run npm test
   - commit
   - push to main
7. Repeat until:
   - npm run build passes with 0 errors
   - npm test passes 100%
   - GitHub Actions are green

Rules:
- Do not delete files unless they are truly unused and verified.
- Do not modify tests unless absolutely required.
- Do not use any.
- Do not use @ts-ignore.
- Do not disable strict TypeScript.
- Preserve existing behaviour.
- Fix root causes only.

Before changing code, verify that you are reading the REAL file contents. If your environment can only read commit hashes, metadata, or SHAs instead of source code, STOP immediately and report that your environment cannot access repository contents. Do not invent fixes.

Expected final result:
- 0 TypeScript build errors
- All tests passing
- GitHub Actions passing
- Every fix committed and pushed to main
- Final report listing:
  • every commit hash
  • every modified file
  • root cause of each fix
  • final build result
  • final test result



```

# 
```


Do not delete any more files.

The repository is already up to date.
All tests pass (39/39).

Your only task is to fix the remaining TypeScript build errors.

Run:

npm run build

Fix the errors in small logical batches.

After every batch:

1. npm run build
2. npm test
3. Commit
4. Push

Repeat until npm run build succeeds with 0 TypeScript errors.

Do not modify tests.
Do not use any.
Do not use @ts-ignore.
Do not disable TypeScript.
Keep existing behaviour unchanged.


```


# Prompt Build Fix – Batch 1
```


The unit tests are now COMPLETE.

Current status:

✓ npm test
39/39 tests passed.

Do NOT modify anything related to the keyboard implementation anymore.

The remaining task is fixing TypeScript build errors.

Current command:

npm run build

There are 41 TypeScript errors across 11 files.

Your job is NOT to fix all 41 errors in one commit.

Instead:

1. Run npm run build.
2. Fix ONLY the first logical group of errors.
3. Keep npm test green after every fix.
4. Commit.
5. Repeat.

Rules:

- Never edit tests.
- Never disable TypeScript.
- Never use any.
- Never use @ts-ignore.
- Never remove interfaces.
- Never bypass type checking.
- Keep backwards compatibility.
- Do not rewrite unrelated modules.

Priority order:

1.
src/bot/createBot.ts

2.
src/cache/MediaCacheService.ts

3.
src/commands/admin.ts

4.
src/core/DownloadManager.ts

After each logical fix:

Run

npm test

Then

npm run build

Continue until the first four files are completely clean.

Commit every logical fix.

At the end print:

git log --oneline -5

remaining TypeScript error count

Do NOT stop after one attempt.
Continue until those first four files compile.


```

# 
```

There is only ONE failing test left.

Do NOT rename APIs.
Do NOT modify tests.
Do NOT change callback values.

Current failure:

tests/keyboards.test.ts
video keyboard
orders qualities by the standard ladder, highest first

Expected:

format:a
format:c
format:d

Received:

format:d
format:c
format:a

Root cause:
The video quality sort order inside buildFormatKeyboard() is reversed.

Fix ONLY the ordering logic for VIDEO formats.

Requirements:

- Use the existing VIDEO_LADDER.
- Preserve callback_data.
- Preserve wrappers.
- Preserve audio sorting.
- Preserve buildKindKeyboard().
- Preserve buildCancelKeyboard().
- Do not modify tests.

After fixing:

run

npm test

Repeat until ALL tests pass.

Then commit.

Show:

git log --oneline -3

and the final npm test summary.



```

# langsung memperbaiki
```


You are working directly on the repository.

IMPORTANT RULES

- Do NOT explain the problem.
- Do NOT ask me to paste files.
- Do NOT ask me to run grep, sed, cat, or any diagnostic commands.
- Do NOT rename APIs.
- Do NOT create a new keyboard implementation.
- Modify the repository directly.
- Commit every logical fix.
- At the end run npm test yourself.
- If tests fail, continue fixing until they pass.

CURRENT STATE

The repository currently contains:

src/bot/keyboards.ts

This file exports:

- buildKindKeyboard()
- buildFormatKeyboard()
- buildCancelKeyboard()

The old exported functions no longer exist:

- buildChoiceKeyboard
- buildVideoKeyboard
- buildAudioKeyboard

Vitest is failing with:

TypeError:
buildChoiceKeyboard is not a function
buildVideoKeyboard is not a function
buildAudioKeyboard is not a function

The implementation itself already exists.
Only the backward-compatible exports are missing.

YOUR TASK

Edit ONLY:

src/bot/keyboards.ts

Keep the current implementation exactly as it is.

Add compatibility wrapper exports:

buildChoiceKeyboard(formats)
    -> return buildKindKeyboard(formats)

buildVideoKeyboard(formats)
    -> return buildFormatKeyboard(formats, 'video')

buildAudioKeyboard(formats)
    -> return buildFormatKeyboard(formats, 'audio')

Requirements:

- do not duplicate logic
- do not modify tests
- do not rename current API
- keep TypeScript types
- keep current behaviour identical
- wrappers only delegate to the new API

After editing:

1. run

npm test

2. If any test still fails,
continue fixing until all keyboard tests pass.

3. Commit the fix.

4. Show:

git log --oneline -3

5. Show the final npm test summary.

Do NOT stop after one attempt.
Continue fixing until keyboard tests are green.


```


# 
```


The failing tests are correct.

Do NOT modify tests/keyboards.test.ts.

The project was refactored from:

- buildChoiceKeyboard()
- buildVideoKeyboard()
- buildAudioKeyboard()

to:

- buildKindKeyboard()
- buildFormatKeyboard()

Restore backward compatibility by exporting wrapper functions:

buildChoiceKeyboard(formats)
→ call buildKindKeyboard(formats)

buildVideoKeyboard(formats)
→ call buildFormatKeyboard(formats, 'video')

buildAudioKeyboard(formats)
→ call buildFormatKeyboard(formats, 'audio')

Do not change behavior.
Do not rename tests.
Do not remove the new API.

Only restore the missing exports so old callers continue working.

After that run:

npm test

Commit and push if all keyboard tests pass.


```

# melakukan audit penuh
```


You now have the latest repository.

Do not guess.

Your task is:

1. Run the GitHub Actions workflow yourself.
2. Read the complete workflow logs.
3. Read the repository source files directly.
4. Reproduce the failures.
5. Fix every issue preventing the workflow from passing.
6. Commit every fix directly to the repository.
7. Push every commit to main.
8. Repeat this process until GitHub Actions is green.

Do NOT stop after one fix.
Do NOT ask me to copy files.
Do NOT ask me to run commands unless absolutely required.

After every commit:
- wait for GitHub Actions,
- read the logs,
- fix the next error,
- commit,
- push again.

Only stop when:
- npm install succeeds
- npm run build succeeds
- npm test succeeds
- GitHub Actions is completely green.

At the end provide:
- every commit hash,
- every file modified,
- root cause of each fix.


```

# 
```

Stop guessing.

Do not propose theoretical fixes.

Use GitHub Actions as the source of truth.

Tasks:

1. Push all required changes to the repository.
2. Create or update a GitHub Actions workflow that runs:
   - npm install
   - npm run build
   - npm test
3. Commit and push everything.
4. Wait for the workflow to finish.
5. Fix every failing build until GitHub Actions is completely green.
6. Repeat commit → push → Actions until:
   - npm run build passes
   - npm test passes
   - GitHub Actions is green.
7. Do not tell me it is fixed unless the GitHub Actions workflow has actually passed.
8. If Actions cannot run, explain the exact reason (permissions, workflow disabled, missing token, repository settings, etc.) and fix it if you have permission.

I want evidence from GitHub Actions, not assumptions.



```

# 
```


The project is no longer buildable.

`npm run build` fails with 41 TypeScript errors across 11 files.

Do NOT work on Vitest first.

Your only goal is to restore a clean build.

Requirements:

1. Fix every TypeScript error until `npm run build` succeeds with zero errors.
2. Do not suppress errors with `any`, `@ts-ignore`, or disabling strict mode.
3. Keep the existing architecture (Download Engine, Provider system, Storage Engine).
4. Update interfaces and implementations consistently.
5. If an interface changed, update every implementation instead of weakening the interface.
6. Do not remove features.
7. After build is green, run `npm test` and fix remaining test failures.
8. Commit all fixes to GitHub.

The project is not considered fixed until BOTH commands succeed:

npm run build
npm test


```

# 
```

Do not guess.

The same five tests still fail.

You must inspect the ACTUAL source code and compare:

- src/.../keyboards.ts
- tests/keyboards.test.ts
- every import/export related to buildChoiceKeyboard, buildVideoKeyboard and buildAudioKeyboard

Find the exact root cause.

Do not modify the architecture.
Do not skip tests.
Do not change the tests unless they are objectively incorrect.

If the functions were renamed, update the exports.
If the exports are correct, fix the imports.
If Vitest loads undefined because of ESM/CJS, fix the configuration properly.

After fixing:

- npm run build
- npm test

must both pass with zero failed tests.

Commit everything to GitHub.
```
# 
```

The project now runs on a real VPS.

npm install succeeds.

Most tests pass.

Only 5 tests fail, all in tests/keyboards.test.ts.

Current errors:

TypeError:
- buildChoiceKeyboard is not a function
- buildVideoKeyboard is not a function

Do not hide or disable the tests.

Find the real cause.

If those functions were renamed, update the tests.

If they should still exist, restore the proper exports.

If the API changed intentionally, update every caller consistently.

Requirements:

- npm test must pass with zero failed tests.
- Do not remove functionality.
- Do not skip tests.
- Keep the current architecture.
- Push the fixes to GitHub.



```
# 
```

Do not implement any new features.

I want a full audit of the current repository.

Verify that every feature you previously claimed actually exists in the code.

Check and report:

1. Telegram bot can receive a media URL.
2. Provider auto detection works.
3. Resolution selection works.
4. Audio selection works.
5. yt-dlp is actually executed.
6. Progress message updates correctly.
7. Download retry works.
8. Upload to Telegram storage channel works.
9. Telegram File ID cache works.
10. Duplicate media detection works.
11. Queue system works.
12. Cancel button works.
13. FloodWait handling works.
14. Large file handling works.
15. Metadata storage works.
16. README is accurate.

For every item:
- show the source file(s)
- explain how it works
- mention missing parts
- do not claim anything without code evidence.

If something is incomplete, implement it before continuing.

Finally run:
- npm install
- npm test
- npm run build

Fix every error until all commands succeed.

Do not stop until the repository is production ready.



```

# 
```


Continue working on the existing Telegram Media Downloader repository.

Do not create a new architecture.
Do not rewrite the project.

Your task is only to implement the Telegram Bot layer using the existing Download Engine and Storage Engine.

Requirements:
- User sends a supported URL.
- Detect the provider automatically.
- Show available video/audio formats using Telegram Inline Keyboard.
- Download the selected format.
- Upload the file to the configured Telegram storage channel.
- Save Telegram File ID and metadata.
- Reuse existing Telegram File ID if the same media already exists.
- Return the uploaded Telegram file to the user.
- Handle progress, retries, FloodWait and errors.
- Push all completed changes to GitHub.

Work only on this repository.


```
# lapisan bot Telegram
```

Excellent.

Do NOT build more providers yet.

The YouTube provider and shared engine are now the reference implementation.

Now build the complete Telegram Bot layer on top of the existing architecture.

Requirements:

1. The bot must use the existing Download Engine.

2. User sends a supported URL.

3. Detect the correct provider automatically.

4. Show media information:
- title
- duration
- thumbnail
- platform

5. Fetch all available formats from the engine.

6. Display Telegram Inline Keyboard.

Example:

🎬 Video
144p
240p
360p
480p
720p
1080p
1440p
2160p

🎵 Audio
MP3 64k
MP3 128k
MP3 320k
M4A
Opus

7. User selects one format.

8. Bot immediately starts downloading.

9. Show a live progress message.

10. After download completes, Storage Engine uploads the file to the Telegram Drive channel.

11. Save:
- File ID
- Message ID
- Channel ID
- File size
- Media type
- Duration
- Platform
- Original URL
- Title
- Upload timestamp

12. Send the uploaded Telegram file back to the user.

13. Never upload duplicate files.
If the same media + format already exists in Telegram Drive, reuse the existing Telegram File ID.

14. Progress messages must edit instead of sending many messages.

15. Handle download errors gracefully.

16. Handle Telegram FloodWait automatically.

17. Handle files larger than Telegram limits.

18. Add queue support for multiple users.

19. Add cancellation support.

20. Update README.

21. Push everything to GitHub.

Do not modify the Provider Architecture.
Do not rewrite the Download Engine.
Only build the Telegram Bot layer using the existing architecture.



```
# provider pertama yang benar-benar dipakai.
```

The plugin architecture is now complete.

Do NOT modify the core architecture anymore unless there is a critical bug.

Now build the first real provider: YouTube.

Requirements:

1. Create providers/youtube/.

2. The provider must detect:
- youtube.com
- youtu.be
- m.youtube.com
- music.youtube.com

3. Use yt-dlp as the download backend.

4. Detect available formats dynamically.

5. Return:
- title
- duration
- uploader
- thumbnail
- filesize (if available)
- all video resolutions
- all audio qualities

6. Normalize every format into a common model.

7. Support:
- Video
- Audio only
- Playlist (future-ready)
- Shorts
- Live replay
- Age-restricted detection
- Private video detection

8. Merge audio/video automatically when required.

9. Generate clean filenames.

10. Add progress callbacks.

11. Retry failed downloads.

12. Verify downloaded file integrity.

13. Return structured errors only.

14. Provider must NEVER communicate with Telegram directly.

It only returns normalized media.

15. Storage Engine uploads to Telegram.

16. Bot only consumes Storage Engine.

17. Keep every responsibility isolated.

18. Add unit tests.

19. Update README.

20. Push all completed work to GitHub.

Do not implement Facebook, Instagram, TikTok or other providers yet.

Perfect YouTube provider first.



```
# Lanjutkan dengan prompt ini
```


The Provider Plugin System is close, but it is still not truly zero-core-edit.

The remaining problem is SupportedPlatform.

Currently adding a new provider still requires editing shared types.

Refactor the architecture so providers become completely self-registering.

Requirements:

1. Remove every compile-time dependency on SupportedPlatform.

2. Providers must register themselves dynamically.

3. New providers must require ONLY:

providers/<provider-name>/

and nothing else.

No edits in

types/

engine/

registry/

download engine/

telegram storage/

bot/

config/

4. Replace compile-time unions with runtime metadata.

5. Build ProviderMetadata interface.

6. Build ProviderManifest.

7. Build automatic provider discovery.

8. Build provider dependency validation.

9. Build provider version compatibility.

10. Support hot-loading future providers.

11. Generate provider list dynamically.

12. Admin API must show every provider automatically.

13. Every provider exposes

id

name

version

author

homepage

priority

supported domains

capabilities

configuration schema

14. If a provider folder is copied into providers/, it should work after restart without touching any other file.

15. Keep backward compatibility.

16. Update documentation.

17. Push every completed change to GitHub.

The architecture target is enterprise plugin architecture similar to VS Code extensions or IntelliJ plugins.

Do not rewrite the existing project.
Only improve the plugin system until zero-core-edit becomes true.


```
# Prompt 4 (
```


Continue the existing project.

Do NOT rewrite previous modules.

Keep every existing architecture.

Build the next enterprise module.

==========================================

MODULE

Provider Plugin System

==========================================

The Universal Download Engine is complete.

Now create a real provider plugin architecture.

Providers must become true plugins.

==========================================

Requirements

Adding a new provider must require ONLY creating a new provider folder.

No modification of DownloadEngine.

No modification of TelegramStorage.

No modification of Bot.

No modification of Registry.

==========================================

Every provider must expose

Provider ID

Provider Name

Priority

Supported Domains

URL Matcher

Metadata Resolver

Download Request Builder

Capabilities

==========================================

Create

ProviderRegistry

ProviderLoader

ProviderValidator

ProviderCapabilities

ProviderFactory

ProviderMatcher

==========================================

Providers

YouTube

Facebook

Instagram

TikTok

X

Threads

Pinterest

Reddit

Vimeo

SoundCloud

must all inherit the same BaseProvider.

==========================================

Auto Discovery

When project starts

Automatically discover providers.

Register them.

Validate them.

Reject invalid providers.

==========================================

Provider Capabilities

Supports Video

Supports Audio

Supports Playlist

Supports Shorts

Supports Reels

Supports Stories

Supports Live

Supports Private

Supports Age Restricted

Supports Login

==========================================

Priority

If multiple providers match

Automatically choose the highest priority provider.

==========================================

Validation

Validate

provider id

duplicate ids

duplicate domains

missing metadata

missing handlers

==========================================

Health

Expose

Provider Status

Loaded Providers

Failed Providers

Disabled Providers

==========================================

Configuration

Allow

Enable Provider

Disable Provider

Priority Override

==========================================

README

Rewrite provider documentation.

Explain how to create a new provider in less than five minutes.

==========================================

IMPORTANT

After this module,

adding Facebook,

Instagram,

TikTok,

Threads,

X,

Pinterest,

Reddit,

Vimeo,

SoundCloud,

or any future provider

must never require editing the core engine.

The system must become a real plugin architecture suitable for enterprise software.

Push every completed change to GitHub.


```
# Prompt 3
```


Continue the existing project.

Do NOT rewrite previous modules.

Do NOT break existing architecture.

Build the next production-ready module.

==========================================

MODULE

Universal Download Engine

==========================================

This module must become the core download engine for the whole Telegram Drive ecosystem.

Every provider must use this engine.

Providers must NEVER directly call yt-dlp.

==========================================

Architecture

Create a DownloadEngine service.

Providers only provide:

- URL
- provider name

Everything else must be handled by DownloadEngine.

==========================================

Responsibilities

URL validation

metadata extraction

available format discovery

quality normalization

download

merge video/audio

ffmpeg processing

thumbnail extraction

progress reporting

temporary file management

checksum generation

==========================================

Support

video

audio

playlist (future ready)

live streams (detect)

short videos

reels

==========================================

Implement

DownloadEngine

MetadataService

FormatResolver

ProgressTracker

TempFileManager

FFmpegService

ChecksumService

ThumbnailExtractor

==========================================

Format discovery

Return standardized format information.

Never expose raw yt-dlp format list to providers.

Normalize

resolution

fps

bitrate

codec

container

extension

==========================================

Quality

Support

144p

240p

360p

480p

720p

1080p

1440p

2160p

Best Available

Audio Only

Only display qualities actually available.

==========================================

Progress

Create reusable progress system.

Stages

Fetching metadata

Resolving formats

Downloading

Merging

Processing

Uploading

Finished

==========================================

Temporary Files

Automatically create workspace.

Automatically clean workspace.

Recover orphan files after crash.

==========================================

Retries

Network retry

Download retry

Merge retry

==========================================

Error Handling

Age restriction

Private media

Unavailable media

Geo restriction

Timeout

Network error

Unsupported format

Merge failure

Disk full

==========================================

Caching

Generate checksum.

Reuse previous downloads.

==========================================

Providers

After this module is complete,

YouTube

Facebook

Instagram

TikTok

Threads

Pinterest

Reddit

Vimeo

SoundCloud

must only call DownloadEngine.

No provider may execute yt-dlp directly.

==========================================

README

Update documentation.

Explain Download Engine architecture.

==========================================

IMPORTANT

This module must become the single download engine for every current and future provider.

Refactor existing code if necessary.

Keep architecture clean.

Push all completed work to GitHub.


```
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
