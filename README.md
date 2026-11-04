# BeeSMART V3 Rev3 — GAS migration foundation

Paket ini dibuat dengan membaca arsip yang diberikan. SQL acuan ditemukan di `config/beesmartv3_rev3.sql` dan berisi **22 tabel**. File ZIP utama juga memuat banyak PHP; paket ini belum mengklaim seluruh 688 PHP telah dikonversi.

## Isi
- `Code.gs`: pembuatan spreadsheet otomatis, sheet sesuai skema Rev3, API CRUD dasar, login MD5 kompatibel untuk `cbt_user`.
- `schema.json`: nama tabel, kolom, tipe SQL, dan primary key yang diekstrak dari dump Rev3.
- `index.html`, `style.css`, `app.js`: halaman konektivitas awal, bukan UI asli BeeSMART.

## Membuat database otomatis
1. Buka https://script.google.com/ dan buat project baru.
2. Salin isi `Code.gs` ke file Code.gs. (File berisi konstanta SCHEMA lengkap.)
3. Jalankan `setupDatabase()` dari editor Apps Script dan berikan izin.
4. Buka Execution log untuk memperoleh URL dan ID Spreadsheet.
5. Jalankan `testDatabase()` untuk pemeriksaan.
6. Deploy > New deployment > Web app. Execute as: Me. Akses: sesuaikan kebutuhan sekolah. Jangan publikasikan database atau endpoint sensitif tanpa pengamanan.
7. Tempel URL `/exec` pada frontend untuk cek kesehatan.

## Catatan penting sebelum produksi
- `setupDatabase()` membuat struktur sheet dan header, **tidak mengimpor seluruh INSERT/data lama** dari SQL. Impor data memerlukan konversi SQL INSERT secara terkontrol dan validasi relasi.
- Login `cbt_user` memverifikasi MD5 karena kode PHP Rev3 menggunakan MD5. MD5 lemah; rencanakan migrasi hash yang lebih kuat setelah kompatibilitas login diverifikasi.
- API CRUD ini adalah fondasi generik, bukan implementasi semua alur bisnis BeeSMART. Jangan dipakai untuk ujian aktif/produksi sebelum seluruh modul diuji.
- Apps Script/Spreadsheet memiliki kuota, batas eksekusi, dan keterbatasan transaksi/locking. Menyimpan setiap jawaban ujian ke Spreadsheet dapat menjadi bottleneck saat peserta serentak.
- Endpoint Apps Script lintas domain dan autentikasi harus diuji pada domain Vercel. Hindari menyimpan token rahasia di GitHub/frontend.
- Aset asli dan template UI tidak disertakan di paket starter ini. Untuk mempertahankan tampilan, tahap berikutnya harus memigrasikan halaman PHP per modul menjadi HTML/JS dan mengganti query MySQL dengan API GAS.

## Inventaris sumber
- Aplikasi berada dalam nested ZIP `Beesmart v3 rev3 Fix Media Player`.
- Dump database Rev3: `config/beesmartv3_rev3.sql`.
- Ada juga dump e-learning Rev2 terpisah; sesuai keputusan, bukan sumber skema utama.
