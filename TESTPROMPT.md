







```
Rapikan UI setelah proses download selesai.

1. Hapus pesan teks:
"Pilih aksi berikutnya:"

Jangan tampilkan lagi karena tidak diperlukan.

2. Keyboard hasil download diubah menjadi vertikal (1 tombol per baris), bukan horizontal.

Susunan harus seperti ini:

☁️ Upload ke Telegram Drive

🔄 Download Lagi

❌ Tutup

Upload dan Download Lagi jangan berada dalam satu baris. Tutup berada di baris paling bawah agar tidak mudah terpencet.

3. Jangan mengubah fungsi tombol. Hanya ubah layout keyboard menjadi lebih rapi dan mudah digunakan di layar HP.

4. Pastikan setelah file berhasil dikirim, yang tersisa di chat hanya:
- Preview video
- File hasil download
- Keyboard vertikal di bawah file

Tidak ada pesan tambahan seperti "Pilih aksi berikutnya".

5. Pastikan semua callback, session, Telegram Drive, downloader, retry, queue, cache, dan seluruh fitur lama tetap berfungsi tanpa perubahan. Build harus sukses tanpa error TypeScript.

```
```
Refactor hanya UX progress download Telegram Bot. Jangan mengubah downloader engine, yt-dlp, Telegram Drive, queue, retry, cache, duplicate detection, upload flow, session, callback flow, maupun logika download yang sudah berjalan.

Tujuan utama: setelah user memilih resolusi (1080p, 720p, dll), bot tidak boleh terlihat diam.

Implementasikan fallback progress message yang stabil:

1. Kurang dari 1 detik setelah user memilih resolusi, bot WAJIB langsung mengirim SATU pesan progress.

Contoh awal:

⏳ Memulai download...

□□□□□□□□□□ 0%

Sedang menyiapkan downloader...

2. Gunakan hanya SATU pesan progress.

Jika preview video tidak bisa diedit oleh Telegram, otomatis buat SATU pesan progress baru, lalu edit pesan itu terus selama proses berjalan.

Jangan membuat banyak pesan baru.

3. Progress harus diperbarui selama proses.

Contoh:

⏳ Download berjalan...

██░░░░░░░░ 20%
🔍 Mengambil metadata...

↓

⏳ Download berjalan...

█████░░░░░ 50%
⬇️ Mengunduh video...

↓

⏳ Download berjalan...

████████░░ 80%
📦 Menyiapkan file...

↓

⏳ Download berjalan...

██████████ 100%
📤 Mengirim ke Telegram...

Tambahkan animasi sederhana yang berubah setiap beberapa detik, misalnya:

⏳
⌛
⏳
⌛

atau

.
..
...
....

agar user tahu bot masih bekerja.

4. Jika proses lebih dari 10 detik, edit pesan progress menjadi:

⏳ Server sedang memproses video.

Video besar dapat memerlukan waktu 30–60 detik.

Mohon jangan menekan tombol lagi.

5. Selama download berjalan, aktifkan anti double click.

Jika user menekan resolusi lagi:

Jangan membuat download kedua.

Balas:

⏳ Download masih berjalan.
Silakan tunggu hingga selesai.

6. Setelah download selesai:

- Hapus pesan progress.
- Kirim file video.
- Setelah file berhasil dikirim, tampilkan keyboard:

☁️ Upload ke Telegram Drive
🔄 Download Lagi
❌ Tutup

Sehingga chat hanya berisi preview dan hasil download tanpa banyak pesan status.

7. Jika download gagal:

Jangan kirim pesan error baru.

Edit pesan progress menjadi:

❌ Download gagal.

Alasan:
<error>

Keyboard:

🔄 Coba Lagi
❌ Tutup

8. Tambahkan logging:

[Progress] created
[Progress] updated
[Progress] edited
[Progress] removed

agar mudah di-debug.

Pastikan build berhasil, tidak ada error TypeScript, seluruh test tetap lolos, dan semua fitur lama tetap berfungsi tanpa regresi.

```
```
# Prompt: Download Progress & Processing UI

Project: telegram-media-downloader

Refactor UX saat proses download agar user selalu mengetahui bot masih bekerja.

Jangan mengubah downloader engine.

Jangan mengubah provider.

Jangan mengubah queue.

Jangan mengubah proses upload Telegram.

Hanya ubah tampilan status proses.

---

## 1. Setelah user memilih kualitas

Misalnya user memilih:

1080p

Bot langsung:

- Disable Inline Keyboard.
- Edit pesan preview.
- Tampilkan status:

⏳ Memulai download...

Sedang mengambil informasi video...

Mohon tunggu.

Jangan diam tanpa respon.

---

## 2. Progress Bertahap

Selama downloader berjalan, update pesan menggunakan editMessageText.

Contoh:

🟡 Menghubungi server...

atau

📋 Mengambil metadata...

↓

🎬 Memilih format terbaik...

↓

⬇️ Mulai download...

↓

██████░░░░ 60%

1.8 MB / 3.4 MB

↓

██████████ 100%

Menggabungkan video...

↓

📦 Menyiapkan file...

↓

📤 Mengirim ke Telegram...

↓

✅ Download selesai.

---

## 3. Jika downloader tidak memiliki progress

Gunakan animasi status.

Contoh berganti setiap 2 detik:

⏳ Memproses.

⏳ Memproses..

⏳ Memproses...

⏳ Memproses....

Lalu ulangi.

User harus selalu melihat bahwa bot masih aktif.

---

## 4. Tahapan Status

Gunakan status berikut sesuai proses sebenarnya:

🔍 Membaca link

📋 Mengambil metadata

🎬 Menentukan kualitas

⬇️ Download video

🎵 Download audio

🔄 Menggabungkan video

📦 Menyiapkan file

📤 Mengirim ke Telegram

☁️ Upload ke Drive (jika dipilih)

✅ Selesai

---

## 5. Timeout

Jika proses lebih dari 10 detik:

Tambahkan:

"Video sedang diproses.

Jangan kirim link lagi.

Bot akan mengirim file secara otomatis setelah selesai."

---

## 6. Error

Jika gagal:

❌ Download gagal.

Alasan:

...

Keyboard:

🔄 Coba Lagi

❌ Tutup

---

## 7. Setelah file berhasil dikirim

Hapus pesan progress.

Yang tersisa hanya:

- Preview
- File video
- Tombol:

☁️ Upload ke Telegram Drive

🔄 Download Lagi

❌ Tutup

Tidak boleh ada progress yang tertinggal.

---

## 8. Anti Double Download

Selama proses berlangsung:

- Tolak request download baru dari user yang sama.
- Jika user mengirim link lain, balas:

"⏳ Download sebelumnya masih berjalan.

Silakan tunggu hingga selesai."

Setelah proses selesai atau gagal, session dibuka kembali.

---

Pastikan semua update menggunakan editMessageText/editMessageReplyMarkup agar chat tetap bersih dan profesional.

```

```
# Prompt: Refactor UI/UX Bot Downloader (Main Menu + Clean Download Flow)

Project: telegram-media-downloader

Lakukan refactor pada UI/UX bot Telegram agar tampil lebih profesional, rapi, dan mudah digunakan.

## Aturan

- Jangan menghapus fitur yang sudah ada.
- Jangan mengubah downloader engine.
- Jangan mengubah provider.
- Jangan mengubah queue.
- Jangan mengubah cache.
- Jangan mengubah duplicate detection.
- Jangan mengubah upload Telegram Drive.
- Jangan mengubah logging.
- Fokus hanya pada alur interaksi (UI/UX) bot.

---

# 1. Main Menu (Reply Keyboard)

Bot menggunakan Reply Keyboard yang selalu muncul di bawah chat (bukan hanya Inline Keyboard dan bukan hanya command /start).

Keyboard utama:

⬇️ Download Video

📂 Telegram Drive

📖 Petunjuk

❓ Bantuan / FAQ

📊 Status Bot

⚙️ Pengaturan

Keyboard ini selalu tampil setelah bot aktif.

---

# 2. /start

Saat user mengetik /start atau pertama kali membuka bot:

Bot menampilkan ucapan singkat.

Contoh:

👋 Selamat datang di Media Downloader.

Bot ini dapat mendownload video dari YouTube, TikTok, Facebook, Instagram, X, dan platform lainnya.

Silakan pilih menu di bawah.

Setelah itu tampilkan Reply Keyboard utama.

Jangan langsung meminta link.

---

# 3. Menu Download Video

Saat user menekan:

⬇️ Download Video

Bot membalas:

Silakan kirim link video yang ingin didownload.

State download dimulai.

---

# 4. User Mengirim URL

Gunakan seluruh downloader yang sudah ada.

Jangan mengubah provider.

Jangan mengubah proses download.

Deteksi provider seperti sekarang.

---

# 5. Preview Media

Bot mengirim SATU pesan preview berisi:

Thumbnail

Judul

Platform

Durasi

Uploader

View (jika ada)

Kemudian tampilkan Inline Keyboard:

🎬 MP4

🎵 MP3

❌ Batal

---

# 6. Jika memilih MP4

JANGAN kirim pesan baru.

Edit Inline Keyboard yang sama menjadi:

1080p

720p

480p

360p

⬅️ Kembali

❌ Batal

---

# 7. Jika memilih MP3

Edit keyboard menjadi pilihan bitrate:

320 kbps

192 kbps

128 kbps

⬅️ Kembali

❌ Batal

---

# 8. Saat user memilih kualitas

Hapus keyboard kualitas.

Edit pesan menjadi:

⏳ Sedang mendownload...

Tambahkan progress yang sudah ada jika tersedia.

Jangan membuat banyak pesan progress.

Gunakan editMessageText atau editMessageReplyMarkup.

---

# 9. Setelah Download Berhasil

Progress selesai.

Kirim file hasil download.

Setelah file berhasil dikirim tampilkan SATU pesan:

Pilih aksi berikutnya:

Dengan Inline Keyboard:

☁️ Upload ke Telegram Drive

🔄 Download Lagi

❌ Tutup

Jangan tampilkan keyboard kualitas lagi.

---

# 10. Upload ke Telegram Drive

Saat tombol ditekan:

Upload menggunakan engine yang sudah ada.

Jika file sudah ada:

Jangan upload ulang.

Gunakan duplicate detection.

Tampilkan:

✅ File sudah tersedia di Telegram Drive.

Jika file baru:

Tampilkan:

✅ Upload berhasil.

Nama File

Ukuran

File ID

Link Telegram Drive (jika tersedia)

Keyboard:

📂 Buka File

🔄 Download Lagi

❌ Tutup

---

# 11. Download Lagi

Saat ditekan:

Reset session.

Hapus state sebelumnya.

Bot membalas:

Silakan kirim link video yang ingin didownload.

Tanpa perlu mengetik /start.

---

# 12. Tutup

Saat ditekan:

Hapus Inline Keyboard.

Tidak menghapus file download.

Tidak menghapus preview.

Tidak menghapus histori chat.

Yang tersisa hanya:

Preview

File download

Informasi upload (jika ada)

---

# 13. Petunjuk

Saat user menekan:

📖 Petunjuk

Bot menjelaskan:

• Cara download video.

• Cara memilih kualitas.

• Cara upload ke Telegram Drive.

• Platform yang didukung.

• FAQ singkat.

---

# 14. Telegram Drive

Saat user menekan:

📂 Telegram Drive

Tampilkan submenu:

📁 File Saya

🔍 Cari File

📊 Statistik

⬅️ Kembali

Gunakan fitur yang sudah ada.

---

# 15. Bantuan

Saat user menekan:

❓ Bantuan / FAQ

Tampilkan:

Cara penggunaan

Masalah umum

Kontak Admin (jika tersedia)

---

# 16. Status Bot

Tampilkan:

Versi Bot

Jumlah Provider Aktif

Status Downloader

Status Telegram Drive

---

# 17. Pengaturan

Sediakan pengaturan sederhana:

🌐 Bahasa

🗑 Bersihkan Session

⬅️ Kembali

---

# 18. Logging

Tambahkan log berikut:

START

MAIN_MENU

HELP_OPENED

DOWNLOAD_STARTED

PROVIDER_DETECTED

FORMAT_SELECTED

DOWNLOAD_PROGRESS

DOWNLOAD_COMPLETED

UPLOAD_STARTED

UPLOAD_COMPLETED

UPLOAD_DUPLICATE

SESSION_RESET

MENU_CLOSED

---

# 19. UI Rules

- Hindari spam pesan.
- Gunakan editMessageText dan editMessageReplyMarkup selama memungkinkan.
- Keyboard kualitas harus hilang setelah dipilih.
- Progress hanya satu pesan.
- Setelah selesai, chat terlihat bersih.
- Keyboard utama (Reply Keyboard) tetap selalu tersedia di bagian bawah chat.
- Semua callback modular.
- Semua state dibersihkan dengan benar.
- Tidak boleh ada breaking change terhadap fitur downloader yang sudah ada.

```
```

Project: telegram-media-downloader

Lakukan refactor alur bot Telegram agar lebih sederhana, cepat, dan profesional.

JANGAN menghapus fitur yang sudah ada.
JANGAN mengubah downloader engine.
JANGAN mengubah queue, cache, duplicate detection, upload Telegram, logging, maupun provider.
Hanya ubah alur interaksi bot.

Target flow:

/start

Bot menampilkan pesan singkat dan Inline Keyboard:

📖 Petunjuk
⬇️ Download Video

=================================

1. Tombol 📖 Petunjuk

Saat ditekan tampilkan:

- Cara menggunakan bot.
- Platform yang didukung.
- Cara memilih kualitas.
- Cara upload ke Telegram Drive.
- Catatan bahwa YouTube kadang membutuhkan cookies yang valid.

Tambahkan tombol:

⬇️ Mulai Download

=================================

2. Tombol ⬇️ Download Video

Bot meminta user mengirim URL.

Contoh:

"Silakan kirim link video yang ingin didownload."

=================================

3. Setelah user mengirim URL

Gunakan seluruh alur downloader yang sudah ada.

Deteksi provider.

Ambil metadata.

Tampilkan pilihan kualitas seperti sekarang.

Tidak mengubah proses ini.

=================================

4. Setelah user memilih kualitas

Bot download file.

Setelah download selesai:

✅ Kirim file langsung ke user.

JANGAN langsung upload ke Telegram Drive.

=================================

5. Setelah file berhasil dikirim

Tampilkan Inline Keyboard baru:

☁️ Upload ke Telegram Drive
🔄 Download Lagi
❌ Tutup

=================================

6. Jika user memilih

☁️ Upload ke Telegram Drive

Baru lakukan proses upload yang sudah ada sekarang.

Gunakan upload engine yang sudah ada.

Setelah berhasil tampilkan:

✅ Upload berhasil

Nama File
Ukuran
File ID
Link Telegram Drive (jika ada)

=================================

7. Duplicate Detection

Jika hash file sudah ada di Telegram Drive:

JANGAN upload ulang.

Langsung gunakan file yang sudah ada.

Tampilkan:

"File sudah tersedia di Telegram Drive."

Lalu tampilkan tombol:

📂 Buka File
🔄 Download Lagi

=================================

8. Download Lagi

Menghapus state sebelumnya.

Bot kembali meminta URL baru.

=================================

9. Tutup

Menghapus keyboard.

Tidak menghapus pesan video.

=================================

10. Logging

Tambahkan log:

START_MENU

HELP_MENU

DOWNLOAD_REQUEST

DOWNLOAD_SUCCESS

UPLOAD_REQUEST

UPLOAD_SUCCESS

UPLOAD_SKIPPED_DUPLICATE

SESSION_RESET

=================================

Pastikan:

- Tidak ada breaking change.
- Semua callback tetap modular.
- Semua state dibersihkan setelah selesai.
- Gunakan SessionManager yang sudah ada.
- Jangan menambah dependency baru.
- Semua fitur lama tetap berjalan.
```

```
Lanjutkan implementasi tahap berikutnya.

Targetnya bukan lagi memperbaiki bug, tetapi meningkatkan tingkat keberhasilan download YouTube pada video yang memunculkan "Sign in to confirm you're not a bot".

Tambahkan Ultimate Fallback Layer.

Tugas:

1. Implementasikan multi extractor strategy.
   Jika extractor utama gagal, coba extractor atau client lain yang masih didukung yt-dlp.

2. Tambahkan adaptive client selection.
   Simpan statistik client mana yang paling sering berhasil dan prioritaskan client tersebut untuk request berikutnya.

3. Tambahkan IP reputation detection.
   Jika error mengindikasikan IP datacenter dibatasi YouTube, beri log khusus agar mudah dibedakan dari error kode.

4. Tambahkan proxy interface.
   Jangan aktifkan proxy secara default, tetapi siapkan arsitektur agar nanti mudah menambahkan HTTP, SOCKS5, atau Residential Proxy tanpa mengubah engine utama.

5. Tambahkan konfigurasi fallback melalui environment variable sehingga urutan client, retry, cookies, dan proxy dapat diubah tanpa mengubah source code.

6. Pastikan semua perubahan tetap kompatibel dengan fitur Telegram Drive, cache, duplicate detection, progress message, dan upload.

7. Setelah selesai jalankan:
   npm run build
   lalu lakukan pengujian ulang dan laporkan:
   - file yang diubah,
   - hasil build,
   - hasil pengujian,
   - apakah masih ada video yang gagal karena pembatasan YouTube.

```
```

Audit dan refactor seluruh engine download YouTube pada project Telegram Media Downloader agar setara dengan downloader profesional.

Jangan hanya memperbaiki satu bug. Audit seluruh alur download YouTube dari awal sampai akhir.

Target:

1. Jangan pernah mengandalkan hanya satu player YouTube.

Buat fallback otomatis seperti berikut:

Android Client
↓
iOS Client
↓
TV Client
↓
Web Creator Client
↓
Web Client
↓
Cookies
↓
PO Token (jika tersedia)

Jika satu client gagal, otomatis lanjut ke client berikutnya tanpa menghentikan proses.

2. Hapus semua hardcode format.

Jangan selalu memakai:

bestvideo+bestaudio

Buat pemilih format dinamis.

Urutan:

- video+audio terbaik
- best
- progressive
- muxed
- adaptive
- format lain yang tersedia

Gunakan hasil --list-formats untuk menentukan pilihan terbaik secara otomatis.

3. Tambahkan retry engine.

Jika yt-dlp gagal:

- coba client lain
- coba format lain
- coba cookies
- coba tanpa cookies
- coba extractor args berbeda

Baru setelah semua gagal kirim pesan gagal.

4. Tambahkan JavaScript Runtime Detection.

Saat startup cek:

node
bun
deno
quickjs

Gunakan runtime terbaik yang tersedia secara otomatis.

Jangan hardcode.

5. Tambahkan dukungan PO Token.

Jika project sudah memiliki PO Token, gunakan otomatis.

Jika tidak ada, lanjut tanpa error.

6. Audit seluruh pemanggilan yt-dlp.

Cari semua file yang menjalankan yt-dlp.

Pastikan command selalu menggunakan opsi terbaik sesuai kondisi.

7. Tambahkan logger lengkap.

Log harus memperlihatkan:

client yang digunakan
format yang dipilih
runtime JS
cookies aktif/tidak
PO Token aktif/tidak
retry ke berapa
alasan retry
format fallback
hasil akhir

8. Refactor engine agar mudah ditambah provider baru.

Pisahkan:

Format Resolver

Client Resolver

Cookie Resolver

Runtime Resolver

PO Token Resolver

Retry Strategy

Download Strategy

Jangan menaruh semua logika dalam satu file.

9. Jangan menghapus fitur Telegram yang sudah ada.

Tetap pertahankan:

progress message

cancel download

Telegram Drive

cache

duplicate detection

upload Telegram

10. Setelah selesai jalankan:

npm run build

Perbaiki semua error sampai build berhasil.

11. Setelah build berhasil lakukan pengujian menggunakan:

YouTube Shorts

YouTube Video biasa

Video umur panjang

Video 4K

Video HDR

Playlist

TikTok

Instagram

Facebook

X/Twitter

Pastikan semuanya tetap berjalan.

PENTING

Jangan hanya memberi patch atau potongan kode.

Kirim SELURUH ISI FILE LENGKAP untuk setiap file yang diubah, dari baris pertama sampai baris terakhir.

Jangan gunakan placeholder "...", "// dst", atau "bagian lain tetap sama".

Jika mengubah 8 file maka kirim lengkap 8 file.

Jika mengubah 20 file maka kirim lengkap 20 file.

Pastikan seluruh kode siap langsung saya salin dan replace tanpa meminta lanjutan atau bagian yang terpotong.
```

```
Audit penuh bug TikTok pada Telegram Media Downloader.

Kondisi saat ini:

- Build sudah berhasil.
- Bot berjalan normal.
- URL vt.tiktok.com sudah di-resolve menjadi URL asli.
- URL yang dikirim ke yt-dlp sekarang adalah:

https://www.tiktok.com/@digital.cell78/video/7652560949013908757

Tetapi yt-dlp masih mengembalikan:

ERROR:
[TikTok] 7652560949013908757: Video not available

Bot kemudian mengirim:

This media is unavailable

Saya tidak ingin workaround.
Saya ingin penyebab sebenarnya ditemukan.

Tugas:

1. Audit seluruh pipeline TikTok mulai dari:
   - URL Resolver
   - TikTok Provider
   - MetadataService
   - YtDlpClient
   - DownloadEngine
   - FFmpegService
   - Telegram Upload

2. Jalankan investigasi pada command yt-dlp yang benar-benar dipakai aplikasi.

3. Cari apakah command yang dikirim ke yt-dlp berbeda dengan command manual.

4. Audit semua argument yt-dlp seperti:
   --extractor-args
   --referer
   --user-agent
   --cookies
   --cookies-from-browser
   --no-playlist
   --dump-single-json
   --no-warnings

5. Cek apakah extractor TikTok sekarang membutuhkan argument baru.

6. Jalankan pembandingan:

yt-dlp "<url>"
yt-dlp -j "<url>"
yt-dlp -F "<url>"

serta command yang dipakai aplikasi.

Bandingkan hasilnya.

7. Jika URL tersebut sebenarnya masih bisa di-download menggunakan browser atau yt-dlp terbaru, cari penyebab kenapa aplikasi gagal.

8. Jika penyebabnya karena cookies, implementasikan loading cookies otomatis.

9. Jika penyebabnya karena extractor TikTok, perbaiki implementasi.

10. Jika penyebabnya karena URL resolver, perbaiki resolver.

11. Tambahkan logging lengkap:

- command yt-dlp final
- exit code
- stderr
- stdout
- resolved URL
- cookies yang digunakan
- user-agent
- extractor args

12. Setelah selesai jalankan build sampai sukses.

13. Jangan berhenti setelah menemukan satu bug.
Lanjut audit sampai TikTok dapat didownload kembali.

PENTING:

Jangan kirim diff.

Jangan kirim patch.

Jangan kirim potongan kode.

Untuk setiap file yang diubah, kirim SATU FILE LENGKAP (full source code) mulai dari baris pertama sampai terakhir.

Jika ada 8 file berubah, kirim 8 file penuh.

Jangan menggunakan placeholder "...", "// unchanged", atau sejenisnya.

Pastikan source code bisa langsung saya replace tanpa meminta file lanjutan.


```
#
```
Audit pipeline TikTok pada Telegram Media Downloader.

Masalah:

Bot berhasil memanggil yt-dlp tetapi yt-dlp mengembalikan:

ERROR:
[TikTok] Video not available

Bot kemudian mengubahnya menjadi:
UNAVAILABLE_MEDIA
This media is unavailable

Tugas:

1. Audit seluruh alur provider TikTok.
2. Cari penyebab kenapa yt-dlp mengembalikan "Video not available".
3. Cek apakah:
   - URL vt.tiktok.com harus di-resolve menjadi URL asli.
   - cookies.txt tidak digunakan.
   - user-agent salah.
   - extractor TikTok memerlukan header tambahan.
   - yt-dlp terlalu lama versinya.
4. Jalankan perintah berikut dan tampilkan hasilnya:

yt-dlp --version

yt-dlp -F "<url>"

yt-dlp -j "<url>"

5. Jika perlu gunakan:
--cookies cookies.txt
atau
--cookies-from-browser

6. Jika vt.tiktok.com masih dipakai, lakukan HTTP redirect terlebih dahulu hingga memperoleh URL www.tiktok.com/@.../video/... sebelum diberikan ke yt-dlp.

7. Jangan hanya menjelaskan.
Perbaiki source code yang diperlukan.

Berikan FULL SOURCE CODE untuk setiap file yang diubah.
Jangan diff.
Jangan potongan kode.
Satu file penuh setiap kali.


```

#
```Build sekarang gagal pada:

src/downloader/FFmpegService.ts

Error:

TS1005: '}' expected

Lokasi:

src/downloader/FFmpegService.ts
baris 42-43

Parser menemukan:

logger.info({

tetapi object tersebut tidak ditutup.

Tugas:

1. Audit seluruh file FFmpegService.ts.
2. Cari semua:
   - kurung kurawal { }
   - tanda (
   - tanda [
   - object literal
   - logger.info(...)
   - logger.error(...)
   - logger.warn(...)
   - logger.debug(...)
3. Pastikan seluruh kurung seimbang.
4. Kirim ulang FULL FILE FFmpegService.ts dari baris pertama sampai terakhir.
5. Jangan kirim diff.
6. Jangan kirim patch.
7. Jangan kirim potongan kode.
8. Kirim hanya file lengkap yang sudah valid.
9. Setelah diperbaiki jalankan:
   npm run build
10. Jika masih ada error, lanjutkan memperbaiki file berikutnya sampai build benar-benar sukses.

PENTING:
Jangan menganggap hanya satu kurung yang kurang. Audit seluruh file karena kemungkinan ada beberapa syntax yang rusak akibat proses edit sebelumnya.



```


#
```

Masalah belum selesai.

Saya sudah melakukan grep di VPS dan "Shared with me" masih muncul di hasil build.

Jangan hanya mengubah isi komponen.

Tugas:

1. Temukan semua import dan render dari:
   - components/phase6/shared-with-me.tsx
   - components/phase6/collaboration-center.tsx

2. Cari siapa yang masih merender komponen tersebut (misalnya AdvancedFileManager, layout, page, provider, atau wrapper).

3. Hapus render kedua komponen tersebut dari root cause. Jangan disembunyikan dengan CSS.

4. Setelah selesai jalankan:
   npm run build

5. Pastikan perintah berikut tidak lagi menemukan "Shared with me" pada source project:
   grep -R "Shared with me" app components src

6. Commit dan push.

7. Jelaskan file mana yang masih merender komponen lama dan bagaimana perbaikannya.

Jangan berhenti sampai halaman My Files hanya memiliki satu header Telegram Drive.

```

#
```
Kamu menghasilkan file TypeScript yang tidak valid.

Build gagal karena kamu memasukkan teks penjelasan ke dalam source code.

Error menunjukkan bahwa di dalam file:

src/telegram/UploadManager.ts

terdapat kalimat seperti:

"Selesai. 14 file di atas adalah semua file source code..."

Kalimat tersebut bukan kode TypeScript sehingga parser gagal.

Tugas:

1. Audit seluruh file yang kamu kirim.
2. Pastikan TIDAK ADA teks penjelasan, markdown, atau komentar di luar syntax TypeScript.
3. Hapus semua kalimat seperti:
   - Selesai...
   - Setelah mengganti...
   - Jalankan...
   - Bagian berikutnya...
   - File berikut...
   - Penjelasan AI...
4. Kirim ulang FULL FILE src/telegram/UploadManager.ts dari baris pertama sampai terakhir.
5. Setelah itu audit seluruh file lain agar tidak ada teks chat yang ikut masuk ke source code.
6. Jalankan npm run build sampai berhasil.
7. Jangan kirim diff atau patch. Kirim FULL FILE yang valid.


```

#
```
Setelah commit terbaru berhasil di-pull ke VPS (commit 755333a), proses build gagal.

Error:

Error: Debug Failure. False expression.
at parseVariableDeclarationList

Ini bukan error TypeScript biasa, tetapi parser TypeScript gagal membaca syntax.

Saya ingin kamu melakukan audit penuh terhadap commit terakhir.

Tugas:

1. Temukan file pertama yang menyebabkan parser crash.
2. Audit semua file yang berubah pada commit terakhir.
3. Cari:
   - variable declaration yang tidak valid
   - object literal rusak
   - generic TypeScript salah
   - arrow function tidak lengkap
   - merge conflict marker (<<<<, ====, >>>>)
   - karakter ilegal
   - syntax TypeScript yang tidak didukung
4. Temukan file dan nomor baris yang pasti menyebabkan parser gagal.
5. Perbaiki semua syntax error.
6. Jalankan kembali:
   npm run build
   npm test
7. Jangan berhenti sampai build benar-benar berhasil tanpa error.

====================================================

PENTING

JANGAN kirim diff.
JANGAN kirim patch.
JANGAN kirim unified diff.
JANGAN kirim git apply.
JANGAN kirim potongan kode.

Saya bekerja dari VPS dan HP.

Saya ingin langsung mengganti file.

====================================================

Untuk SETIAP file yang diperbaiki atau diubah, kirim FULL FILE.

Format WAJIB:

====================================================
FILE 1

Path:
src/...

Alasan perubahan:

...

```ts
ISI LENGKAP FILE DARI BARIS PERTAMA SAMPAI BARIS TERAKHIR


```

#
```
Jangan membuat patch di workspace internal (/mnt/workspace) dan jangan memberikan perintah git pull atau --allow-unrelated-histories.

Masalahnya bukan di VPS. Repository GitHub saya masih berada pada commit c1eb752, sedangkan perubahan yang kamu buat hanya ada di workspace internal sehingga VPS selalu menampilkan:

- git pull → Already up to date
- git push → Everything up-to-date

Saya TIDAK bisa mengakses /mnt/workspace, jadi patch yang kamu buat di sana tidak berguna.

Yang saya inginkan:

1. Jangan simpan patch di workspace internal.
2. Jangan membuat commit lokal yang tidak bisa di-push.
3. Tampilkan LANGSUNG seluruh perubahan di chat.
4. Untuk setiap file yang diubah, tampilkan:
   - path file
   - isi lengkap file baru, atau unified diff (git diff) lengkap yang bisa saya salin.
5. Jika file baru dibuat, tampilkan isi lengkap file tersebut.
6. Jika file dihapus, jelaskan alasannya.
7. Jangan memotong output. Jika terlalu panjang, kirim dalam beberapa bagian sampai selesai.
8. Jangan menggunakan path internal seperti /mnt/workspace atau /tmp.
9. Jangan menyuruh saya menjalankan git pull lagi karena repository GitHub belum berubah.
10. Fokus pada perubahan source code yang memperbaiki bug MP4/MP3 dan tampilkan seluruh perubahan yang benar-benar diperlukan agar saya bisa menerapkannya langsung ke repository saya.

Jika kamu memang memiliki akses GitHub dan bisa push, tampilkan:
- git remote -v
- git branch
- git status
- git log --oneline -5
- hash commit HEAD
- hasil git push

Jika tidak bisa push, jangan membuat patch internal. Tampilkan seluruh perubahan source code langsung di chat sampai selesai.


```

#
```
Kamu menampilkan commit 0905341, tetapi repository GitHub saya hanya memiliki commit c1eb752.

Di VPS:

- git pull -> Already up to date
- git push -> Everything up-to-date
- origin/main -> c1eb752

Artinya commit 0905341 tidak ada di repository GitHub yang digunakan VPS.

Tolong audit Git yang sedang kamu gunakan.

Tampilkan:
1. git remote -v
2. git branch
3. git status
4. git log --oneline -5
5. commit hash HEAD
6. repository URL yang sedang kamu edit

Jika commit 0905341 belum ada di GitHub, push commit tersebut ke repository:
https://github.com/zenolambetuna/telegram-media-downloader.git

Setelah berhasil push, tampilkan hash commit terbaru yang sudah ada di origin/main.


```


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
