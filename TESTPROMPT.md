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
