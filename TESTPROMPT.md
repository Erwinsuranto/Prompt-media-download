





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
