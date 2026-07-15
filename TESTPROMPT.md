



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
