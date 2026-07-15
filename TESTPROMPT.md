




#
```
# TASK: Refactor Total Media Selection Pipeline

Jangan membuat patch kecil.

Lakukan audit dan refactor penuh terhadap pipeline Telegram Media Downloader mulai dari:

Link
↓
Extractor
↓
Metadata
↓
FormatResolver
↓
MediaTypeKeyboard
↓
ResolutionKeyboard
↓
Callback Handler
↓
Download Queue
↓
Telegram Upload

===========================================================
MASALAH SAAT INI
===========================================================

Untuk link TikTok bot hanya menampilkan:

🎵 Audio
❌ Cancel

Padahal media tersebut adalah VIDEO.

Yang saya inginkan adalah:

Semua media yang memiliki video harus selalu memiliki tombol:

🎥 MP4
🎵 MP3
❌ Cancel

Bukan hanya Audio.

===========================================================
TARGET UI
===========================================================

Semua platform memakai alur yang sama.

Contoh:

User kirim link

↓

Bot membaca metadata

↓

Bot mengirim preview

↓

Bot menampilkan

🎥 MP4
🎵 MP3
❌ Cancel

===========================================================
ALUR MP4
===========================================================

Saat user menekan MP4

IF

platform memiliki banyak resolusi

contoh:

YouTube

maka tampilkan

2160p
1440p
1080p
720p
480p
360p

Cancel

baru download.

IF

platform hanya memiliki satu kualitas

contoh:

TikTok
Instagram
Facebook
X

langsung download kualitas terbaik.

===========================================================
ALUR MP3
===========================================================

Saat user memilih MP3

langsung download audio terbaik.

===========================================================
JANGAN PERNAH
===========================================================

Jangan menyembunyikan tombol MP4 hanya karena:

- thumbnail tidak ada

- metadata kurang lengkap

- hanya ada satu kualitas

- tidak ada daftar resolusi

- codec tertentu

Kalau ada video

MP4 WAJIB muncul.

===========================================================
PIPELINE BARU
===========================================================

Buat model data baru.

interface MediaInfo {

platform

hasVideo

hasAudio

videoFormats

audioFormats

bestVideo

bestAudio

supportsResolutionSelection

}

Semua extractor harus mengembalikan struktur ini.

===========================================================
FORMAT RESOLVER
===========================================================

Audit

src/downloader/FormatResolver.ts

Pastikan

resolve()

menghasilkan

hasVideo=true

jika videoFormats.length > 0

hasAudio=true

jika audioFormats.length > 0

supportsResolutionSelection=true

hanya jika

videoFormats.length > 1

===========================================================
KEYBOARD
===========================================================

Audit

src/bot/keyboards.ts

Hapus logika lama.

Buat fungsi baru:

buildMediaTypeKeyboard()

Hasilnya:

Jika hasVideo

tambahkan

🎥 MP4

Jika hasAudio

tambahkan

🎵 MP3

Selalu

❌ Cancel

Jangan gunakan kondisi lain.

===========================================================
RESOLUTION KEYBOARD
===========================================================

Tambahkan

buildResolutionKeyboard()

Hanya dipakai

jika

supportsResolutionSelection=true

===========================================================
CALLBACK
===========================================================

Audit callback

media:video

media:audio

resolution:*

cancel

Pastikan callback state tetap tersimpan.

===========================================================
PLATFORM
===========================================================

Audit seluruh extractor:

TikTok

YouTube

Instagram

Facebook

Twitter/X

Pinterest

Threads

===========================================================
LOGGING
===========================================================

Tambahkan log:

Platform

hasVideo

hasAudio

videoFormats.length

audioFormats.length

supportsResolutionSelection

Keyboard generated

Selected callback

Selected resolution

Selected format

Download started

Upload started

Upload finished

===========================================================
UNIT TEST
===========================================================

Tambahkan test:

TikTok

harus menghasilkan

hasVideo=true

hasAudio=true

MP4 muncul

MP3 muncul

YouTube

MP4 muncul

MP3 muncul

Resolution muncul

Instagram

MP4 muncul

Facebook

MP4 muncul

X

MP4 muncul

===========================================================
FILE YANG HARUS DIAUDIT
===========================================================

src/downloader/FormatResolver.ts

src/downloader/

src/extractors/

src/bot/keyboards.ts

src/bot/callbacks.ts

src/bot/createBotApplication.ts

src/download/

src/types/

===========================================================
OUTPUT
===========================================================

Jangan hanya menjelaskan.

Perbaiki source code.

Tampilkan seluruh file yang diubah.

Jika perlu buat file baru.

Jalankan:

npm run build

npm test

Pastikan seluruh test lulus.

Lalu lakukan pengujian nyata menggunakan:

1 link TikTok

1 link YouTube

1 link Instagram

1 link Facebook

dan tampilkan hasil keyboard yang muncul.

Jangan selesai sebelum tombol MP4 benar-benar muncul untuk semua media video.


```

#
```

Audit dan ubah seluruh alur pemilihan format download pada Telegram Media Downloader.

Target perilaku yang diinginkan:

1. Setelah pengguna mengirim link (TikTok, YouTube, Facebook, Instagram, X, dll), bot JANGAN langsung menampilkan tombol MP3 saja.

2. Jika media memiliki VIDEO, tampilkan keyboard:

🎥 MP4
🎵 MP3
❌ Cancel

3. Jika pengguna menekan MP4:
- Bila platform memiliki beberapa resolusi (misalnya YouTube), tampilkan daftar resolusi terlebih dahulu, misalnya:
  - 2160p
  - 1440p
  - 1080p
  - 720p
  - 480p
  - 360p
  - Cancel
- Setelah pengguna memilih resolusi, baru mulai download.

4. Jika platform tidak menyediakan pilihan resolusi (misalnya sebagian besar TikTok, Facebook, Instagram, X), setelah tombol MP4 ditekan bot langsung mendownload video kualitas terbaik tanpa menampilkan menu resolusi.

5. Jika pengguna menekan MP3, langsung download audio.

6. Jangan pernah menampilkan tombol MP3 saja jika video tersedia.

7. Seluruh platform harus menggunakan alur yang sama:
Link → Pilih MP4 / MP3 / Cancel → (Jika perlu pilih resolusi) → Download.

8. Refactor kode agar keyboard dibuat dari satu komponen bersama (shared keyboard builder), bukan logika terpisah untuk setiap platform.

9. Audit callback query dan state management agar semua tombol berfungsi.

10. Setelah selesai lakukan pengujian nyata pada:
- TikTok
- YouTube
- Facebook
- Instagram
- X

Pastikan:
✓ TikTok menampilkan MP4, MP3, Cancel.
✓ YouTube menampilkan MP4, MP3, Cancel lalu pilihan resolusi sebelum download video.
✓ Platform tanpa pilihan resolusi langsung mendownload setelah MP4 ditekan.
✓ Semua callback bekerja dengan benar.

Jangan hanya menjelaskan. Langsung ubah source code dan tampilkan file yang diubah beserta hasil pengujiannya.

```


#
```
Audit dan perbaiki bug pada Telegram Media Downloader.

Masalah:
- Saat pengguna mengirim link TikTok, bot hanya menampilkan tombol "MP3".
- Tombol "Video / MP4" sama sekali tidak muncul.
- Akibatnya pengguna tidak bisa memilih download video.

Tugas:
1. Audit seluruh alur setelah link TikTok diterima.
2. Periksa fungsi yang membuat inline keyboard.
3. Cari penyebab kenapa tombol Video/MP4 tidak dibuat atau tidak ditampilkan.
4. Pastikan jika media memiliki video maka tombol berikut selalu muncul:
   - 🎥 Video (MP4)
   - 🎵 Audio (MP3)
5. Jangan sembunyikan tombol Video hanya karena thumbnail atau metadata tertentu gagal dibaca.
6. Audit callback_data tombol Video apakah benar terdaftar dan diproses.
7. Periksa parser TikTok, YouTube, Instagram, Facebook, X, dan platform lain agar semuanya menampilkan pilihan Video jika tersedia.
8. Setelah diperbaiki, lakukan pengujian dan pastikan:
   - Link TikTok → muncul tombol Video dan MP3.
   - Tombol Video dapat ditekan dan memulai download.
   - Tombol MP3 tetap berfungsi.

Jangan hanya menjelaskan penyebabnya. Langsung perbaiki source code, tampilkan file yang diubah, dan jelaskan mengapa tombol Video tidak muncul.


```


#
```

Tolong audit dan perbaiki bug pada project Telegram Media Downloader.

Masalah:
- Download video TikTok gagal.
- Setelah memilih format video, bot mencoba upload ke Telegram tetapi selalu muncul pesan:
  "Upload failed after retries"
- MP3 masih bisa, tetapi video gagal.
- Tombol video tidak berfungsi dengan benar.

Lakukan audit menyeluruh pada seluruh alur download:

1. Audit proses pengambilan URL video dari TikTok.
2. Audit proses download file video.
3. Audit ukuran file sebelum upload.
4. Audit stream download agar tidak korup.
5. Audit upload ke Telegram (sendVideo/sendDocument).
6. Audit timeout upload.
7. Audit retry mechanism karena saat ini retry tetap gagal.
8. Audit apakah file sementara (temp file) terhapus sebelum upload selesai.
9. Audit penggunaan ffmpeg jika video diproses terlebih dahulu.
10. Audit permission folder temp dan folder download.
11. Audit semua error yang ditangkap try/catch, tampilkan log lengkap.
12. Audit apakah upload gagal karena Telegram API, ukuran file, atau koneksi.

Setelah menemukan penyebabnya:
- Perbaiki kode secara langsung.
- Jangan menghapus fitur yang sudah berjalan.
- Pastikan video TikTok berhasil dikirim ke Telegram.
- Pastikan YouTube, Facebook, Instagram, X, dan platform lain tetap berfungsi.
- Tambahkan logging yang jelas agar jika upload gagal dapat diketahui penyebab pastinya.
- Berikan ringkasan file yang diubah beserta alasan setiap perubahan.

```

#
```
Kerjakan langsung di repository Telegram Drive. Jangan berhenti di tahap analisis. Ubah kode sampai masalah benar-benar selesai.

Masalah yang harus diperbaiki:

Pada halaman "My Files" masih muncul header/template lama dengan tombol:
- Shared with me
- Collaborate

Padahal halaman seharusnya hanya menampilkan header Telegram Drive.

Tugas:

1. Cari sumber komponen yang menampilkan "Shared with me" dan "Collaborate".
2. Audit seluruh project, terutama:
   - app/
   - components/
   - layouts/
   - providers/
   - templates/
   - src/
3. Temukan penyebab utama (root cause) kenapa header lama masih dirender bersama header Telegram Drive.
4. Hapus header/template lama dari akar masalah, bukan disembunyikan menggunakan CSS.
5. Pastikan hanya ada SATU header Telegram Drive pada desktop maupun mobile.
6. Periksa apakah ada layout bertumpuk, nested layout, provider, wrapper, atau komponen yang dirender dua kali, lalu perbaiki.
7. Jangan menghapus fitur Telegram Drive seperti:
   - Upload
   - My Files
   - New Folder
   - Search
   - Download
   - Navigation
8. Hapus kode, import, dan komponen template lama yang sudah tidak digunakan.
9. Jalankan:
   npm run build
   npm start
10. Pastikan:
   - Tombol "Shared with me" sudah hilang.
   - Tombol "Collaborate" sudah hilang.
   - Tidak ada header ganda.
   - Tampilan My Files tetap normal.
11. Simpan semua perubahan.
12. Commit dengan pesan:
    Fix duplicate template header on My Files
13. Push ke branch saat ini.

Jangan berhenti setelah menemukan penyebabnya. Lanjutkan sampai masalah benar-benar selesai, lalu laporkan:
- Penyebab utama (root cause).
- File yang diubah.
- Perubahan yang dilakukan.
- Hasil pengujian setelah diperbaiki.


```

#
```
The TikTok provider is still incorrect.

Current behavior:
- Telegram callback works.
- The MP3 button works.
- The bot attempts to upload.
- But for every TikTok URL, only one format is detected:
  total=1
  video=0
  audio=1

Expected behavior:
The bot must detect all available TikTok video formats and show them in the Telegram keyboard, for example:
- MP4 1080p
- MP4 720p
- MP4 480p
- MP3

Do NOT add more debug logs.

Find the real root cause and fix it.

Requirements:
1. Trace the complete pipeline:
   YtDlpClient
   → MetadataService
   → FormatResolver
   → MediaFormat mapping
   → Keyboard generation

2. Verify that yt-dlp is executed correctly for TikTok.
   If the command or arguments cause yt-dlp to return only audio, fix them.

3. Inspect the raw JSON returned by yt-dlp.
   If the JSON already contains video formats but they disappear later, fix the code where they are filtered or mapped.

4. If MetadataService removes video formats, fix it.

5. If FormatResolver incorrectly classifies TikTok formats as audio-only, fix it.

6. If TikTok requires a different extraction strategy than YouTube, implement a dedicated TikTok parser while keeping other providers unchanged.

7. Preserve merge support for separate video/audio streams.

8. Do not hardcode resolutions.
   Generate the keyboard dynamically from the formats returned by yt-dlp.

9. After the fix, verify that a normal TikTok URL displays video buttons before the MP3 option.

10. Run all tests, build the project, and ensure nothing else breaks.

The objective is NOT to make MP3 work.
The objective is to restore full TikTok video format detection so users can download MP4 videos as well as MP3.


```


#
```

You are working directly inside the Telegram Drive repository. Do not stop at analysis. Modify the code until the problem is completely fixed.

Problem:
The /my-files page renders two headers.
The old template header with "Shared with me" and "Collaborate" is still displayed above the Telegram Drive header.

Tasks:
1. Find the exact component rendering "Shared with me" and "Collaborate".
2. Search the entire project including:
   - app/
   - components/
   - layouts/
   - providers/
   - templates/
   - src/
3. Find why the old template header is rendered together with the Telegram Drive header.
4. Remove the old template header completely. Do NOT hide it with CSS.
5. Keep only one Telegram Drive header across desktop and mobile.
6. Remove any duplicate layouts, providers, wrappers or unused components that cause the duplicate UI.
7. Remove dead code related to the old template if it is no longer used.
8. Run:
   - npm run build
   - npm start
9. Verify that:
   - "Shared with me" is no longer visible.
   - "Collaborate" is no longer visible.
   - Only one Telegram Drive header remains.
   - All existing features (Upload, My Files, Search, Folder, Download, Navigation) continue working.
10. Save all modified files.
11. Commit the changes with message:
    Fix duplicate header on my-files
12. Push the commit to the current branch.

Do not stop after finding the problem. Continue until the duplicate header is completely removed and verified. At the end, list every modified file and explain the root cause.

```


#
```
Audit and fix the entire Telegram Drive project without asking for confirmation. Work directly in the repository and apply all necessary changes.

Current issues:
1. The /my-files page renders two headers. The old template header with "Shared with me" and "Collaborate" is still visible above the Telegram Drive header. Remove the old template completely so only one Telegram Drive header remains.
2. Search all layouts, nested layouts, templates, providers, and components that may render duplicate headers or navigation. Fix the root cause instead of hiding it with CSS.
3. Keep only one responsive header and one navigation system for both desktop and mobile.
4. Preserve all existing Telegram Drive features and styling. Do not remove any functional components except the duplicated template UI.
5. Ensure the Telegram bot starts correctly. The project currently expects CHANNEL_ID while .env.example uses TELEGRAM_CHANNEL_ID. Make the bot accept both variables:
   - CHANNEL_ID
   - TELEGRAM_CHANNEL_ID
   Use whichever is available without breaking backward compatibility.
6. Verify that website startup, bot startup, MongoDB connection, upload, My Files, folder creation, search, download, and channel storage all work after the fix.
7. Remove dead code, unused imports, duplicate layouts, and obsolete template components if they are no longer referenced.
8. Update README.md with a brief changelog describing every fix that was applied.
9. Build the project, fix any TypeScript/ESLint/build errors, and do not stop until the project builds successfully with:
   - npm run build
   - npm start
   - npm run bot

When finished, provide:
- Files changed.
- Root cause of the duplicate header.
- Root cause of the CHANNEL_ID issue.
- Summary of all fixes performed.


```

# Minta Cline fokus ke callback, bukan yt-dlp
```

Stop investigating yt-dlp for now.

The bigger issue is that pressing the Telegram inline button does absolutely nothing.

Current behavior:

- Keyboard is displayed successfully.
- Buttons:
  🎵 Audio
  ❌ Cancel
- Pressing either button produces NO new logs.
- The last log is:
  [DEBUG] Message sent

This means callback_query updates are probably never handled.

Please inspect the COMPLETE Telegram callback flow.

Verify:

1. bot.on("callback_query") or equivalent is registered.
2. CallbackQuery updates are reaching the application.
3. callback_data generated for every button.
4. callback_data matches the parser.
5. answerCallbackQuery() is called.
6. Session lookup works.
7. No exception is swallowed.
8. Add logs at the FIRST line of the callback handler.

Required logs:

[CALLBACK] update received
[CALLBACK] callback_data
[CALLBACK] session found
[CALLBACK] selected action
[CALLBACK] download started
[CALLBACK] error

If pressing the button does not even print "[CALLBACK] update received", find why callback_query updates are not being processed and fix the root cause.

Do not continue debugging yt-dlp until callback_query works.

```

#
```


The current debug logs are still incomplete.

I only have:

FormatResolver.resolve()
Mapped MediaFormat[]
metadata.formats
Keyboard buttons

This is too late because the video formats are already gone.

I need debug logs BEFORE FormatResolver.

Please modify the code to log:

1. The COMPLETE raw JSON returned by yt-dlp.
2. The raw formats array immediately after parsing yt-dlp.
3. The raw formats passed into MetadataService.
4. The raw formats passed into FormatResolver.

Do not truncate the formats array.

Then rebuild, run the bot, test the same TikTok URL, and identify exactly where video formats disappear.

Do not guess. Fix the root cause only after locating the exact step.
```
#
```
The Telegram inline keyboard has two issues.

Issue 1:
For TikTok video URLs, the bot only creates one format:
video = 0
audio = 1

Issue 2:
The "🎵 Audio" button is displayed but pressing it does absolutely nothing.
No download starts.

Please debug the COMPLETE callback flow.

Verify all of these:

1. callback_query handler is registered.
2. callback_data generated by buildKindKeyboard().
3. callback_data received from Telegram.
4. session lookup using callback_data.
5. download handler execution.
6. answerCallbackQuery().
7. editMessageReplyMarkup() or sendMessage().
8. any exception swallowed by try/catch.

Add debug logs for every step:

[CALLBACK] received
[CALLBACK] data
[CALLBACK] session found
[CALLBACK] selected format
[CALLBACK] download started
[CALLBACK] download finished
[CALLBACK] error

Do not guess.
Find the exact place where the callback stops and fix the root cause.
Build, run tests, and commit the fix.


```

#
```
The TikTok URL is a VIDEO, but the bot only shows the 🎵 Audio button.

Current debug logs show:

FormatResolver.resolve():
total: 1
video: 0
audio: 1

The keyboard is:
🎵 Audio
❌ Cancel

This is incorrect. The problem is NOT the Telegram keyboard. The problem is somewhere before FormatResolver.

Please perform a complete end-to-end investigation from MetadataService until FormatResolver.

Tasks:
1. Add detailed debug logs immediately after yt-dlp returns metadata.
2. Print the complete raw formats array.
3. For every format print:
   - format_id
   - ext
   - protocol
   - width
   - height
   - resolution
   - fps
   - vcodec
   - acodec
   - dynamic_range
   - format_note
   - filesize
4. Compare the raw yt-dlp formats with the formats passed into FormatResolver.
5. Identify exactly where video formats disappear.
6. If MetadataService, normalization, parser, filtering, or mapping removes video formats, fix the root cause instead of adding workarounds.
7. Preserve audio-only handling for true audio posts, but ensure TikTok video posts expose all available video qualities plus Audio.
8. Add regression tests so this bug cannot happen again.
9. Run all tests, build the project, and verify that a normal TikTok video produces video format buttons (360p/540p/720p/1080p when available) in addition to the Audio button.

Do not stop after adding logs. Continue until the root cause is found, fixed, tested, and verified.


```

#
```
The debug logs identified the root cause.

Current logs show:

FormatResolver.resolve():
total = 1
video = 0
audio = 1

The keyboard is correct. It only displays Audio because no video formats exist.

Do NOT modify keyboards, sessions, or FormatResolver anymore.

Find why the TikTok provider only returns audio.

Trace the pipeline:

yt-dlp JSON
→ provider parser
→ DownloadArtifact
→ inspect()
→ metadata.formats

Add debug logs showing:

- raw yt-dlp formats count
- every format ext
- vcodec
- acodec
- width
- height
- protocol
- format_id

Check whether video formats are filtered incorrectly.

If TikTok returns progressive MP4 with both video and audio, keep it.

If video-only formats exist, keep them.

Do not discard formats because width/height is missing.

The goal is that metadata.formats contains video entries before FormatResolver runs.


```
#
```
The TikTok format resolver has already been fixed and tested.

The VPS is running the latest code.

Problem:
- TikTok preview appears.
- Metadata is correct.
- Only "Audio" and "Cancel" buttons are displayed.
- No "Video" button is created.

This means the bug is NOT in FormatResolver anymore.

Audit the complete callback/button generation flow.

Trace the data step by step:

1. yt-dlp metadata
2. FormatResolver output
3. DownloadArtifact creation
4. Session storage
5. Inline keyboard builder
6. Callback data
7. Telegram reply

For every stage print logs:
- number of video formats
- number of audio formats
- media type
- quality
- callback payload

Find exactly where video formats disappear.

Do NOT guess.

Do NOT add workaround.

Find the first place where the video list becomes empty and fix the root cause.

After fixing:
- run tests
- npm run build
- verify TikTok keyboard contains Video + Audio buttons.


```
#
```

TikTok provider is detected correctly, but the inline keyboard is wrong.

Current behavior:
- Preview is shown.
- Audio button is shown.
- Cancel button is shown.
- Video button is missing.

Audit the entire TikTok download flow.

Check:
1. Provider detection.
2. Metadata extraction.
3. Available formats returned by yt-dlp.
4. Keyboard generation.
5. Why no Video button is created.
6. Why only Audio is displayed.
7. Fix the root cause instead of adding a workaround.

Requirements:
- If video formats exist, always show a Video button.
- If audio exists, show Audio.
- If multiple qualities exist, show Video 1080p / 720p / 480p.
- Keep YouTube, Facebook, Instagram and X behavior unchanged.
- After fixing, run npm run build and verify the keyboard works.

```
```

Lanjutkan audit repository ini sebagai Senior Software Architect.

Jangan mengubah file apa pun.

Lakukan audit mendalam terhadap:

1. Alur lengkap:
   URL → Provider → Download Engine → yt-dlp → ffmpeg → Telegram Upload → Database → User.

2. Cari race condition, deadlock, memory leak, file leak, dan concurrency issue.

3. Audit keamanan:
   - command injection
   - SSRF
   - path traversal
   - file overwrite
   - input validation
   - secret leakage

4. Audit performa:
   - bottleneck
   - database
   - queue
   - cache
   - upload Telegram
   - penggunaan RAM dan CPU

5. Audit maintainability:
   - duplicate code
   - architecture smell
   - dependency issues
   - SOLID
   - extensibility untuk provider baru

6. Cari semua TODO, FIXME, partial implementation, dan fitur yang belum selesai.

Buat laporan baru AUDIT_STAGE2.md dengan prioritas:
Critical
High
Medium
Low
Future Improvement

Jangan memperbaiki apa pun sebelum audit selesai.

```
