# SisaBaik API

SisaBaik merupakan aplikasi berbasis web yang dikembangkan untuk membantu mengurangi pemborosan makanan melalui sistem penawaran makanan berlebih dari penyedia kepada pembeli. Aplikasi ini menyediakan pengelolaan akun, penawaran makanan, dan pemesanan dengan dukungan database PostgreSQL.

## 1. Teknologi yang Digunakan

- **Runtime:** Node.js (minimal versi 22)
- **Backend:** Express.js
- **Database:** PostgreSQL
- **Database Driver:** node-postgres (`pg`)
- **Autentikasi:** JSON Web Token (JWT)
- **Password Hashing:** bcryptjs
- **Pengujian:** Node.js dan HTTP API testing

## 2. Struktur Proyek

```text
SisaBaik-Api/
├── database/
│   ├── 01_schema.sql
│   ├── 02_seed.sql
│   ├── 03_queries.sql
│   ├── 04_crud_safe.sql
│   ├── 05_transaction_order.sql
│   ├── 06_integrity_test.sql
│   └── 07_auth.sql
├── docs/
│   ├── API.md
│   └── hasil-uji-chapter8.md
├── public/
├── scripts/
│   ├── check-db.js
│   ├── create-order.js
│   ├── uji-auth.js
│   ├── uji-api.js
│   ├── uji-konkurensi.js
│   └── verifikasi-penyedia.js
├── src/
│   ├── config/
│   ├── controllers/
│   ├── db/
│   ├── lib/
│   ├── middleware/
│   ├── repositories/
│   ├── routes/
│   └── services/
├── .env
├── .gitignore
├── package.json
└── server.js
```

## 3. Persyaratan Sistem

Pastikan perangkat telah memiliki:

1. Node.js versi 22 atau lebih baru.
2. npm.
3. PostgreSQL.
4. Git.
5. Browser untuk mengakses halaman aplikasi.

## 4. Instalasi Proyek

Clone repository:

```bash
git clone <URL_REPOSITORY>
```

Masuk ke direktori proyek:

```bash
cd SisaBaik-Api
```

Install seluruh dependency:

```bash
npm install
```

## 5. Konfigurasi Environment

Buat file `.env` pada direktori utama proyek.

Nama environment yang digunakan:

| Environment  | Keterangan                                               |
| ------------ | -------------------------------------------------------- |
| `PGHOST`     | Host database PostgreSQL                                 |
| `PGPORT`     | Port PostgreSQL                                          |
| `PGDATABASE` | Nama database                                            |
| `PGUSER`     | Pengguna database                                        |
| `PGPASSWORD` | Kata sandi database                                      |
| `JWT_SECRET` | Kunci rahasia untuk penandatanganan JWT                  |
| `PGSSL`      | Konfigurasi SSL jika diperlukan oleh lingkungan database |

Contoh format `.env` tanpa nilai rahasia:

```env
PGHOST=
PGPORT=
PGDATABASE=
PGUSER=
PGPASSWORD=
JWT_SECRET=
PGSSL=
```

**Catatan keamanan:**

- Isi environment sesuai konfigurasi PostgreSQL masing-masing.
- `JWT_SECRET` harus berupa 64 digit heksadesimal acak.
- Jangan memasukkan nilai rahasia ke README atau repository.
- Jangan mengunggah file `.env` ke Git.
- Pastikan `.env` sudah tercantum dalam `.gitignore`.

## 6. Persiapan Database dan Migrasi

Buat database PostgreSQL dengan nama yang digunakan pada `PGDATABASE`.

Untuk lingkungan praktikum lokal, database yang digunakan adalah:

```text
sisabaik_dev
```

Jalankan migrasi SQL secara berurutan agar struktur database terbentuk sesuai kebutuhan aplikasi.

```bash
psql -h localhost -U postgres -d sisabaik_dev -f database/01_schema.sql
psql -h localhost -U postgres -d sisabaik_dev -f database/02_seed.sql
psql -h localhost -U postgres -d sisabaik_dev -f database/07_auth.sql
```

File migrasi `07_auth.sql` digunakan untuk menyiapkan kebutuhan autentikasi, termasuk struktur data akun dan penyedia sesuai implementasi Chapter 8.

Setelah migrasi selesai, periksa koneksi database:

```bash
npm run
```

Jalankan script pemeriksaan database jika tersedia pada `package.json`.

## 7. Registrasi dan Autentikasi

SisaBaik menyediakan dua jenis pendaftaran mandiri:

### A. Pembeli

Pembeli dapat melakukan pendaftaran dengan informasi:

- Nama lengkap
- Email
- Kata sandi

Akun pembeli dapat digunakan untuk login dan melakukan pemesanan.

### B. Penyedia

Penyedia dapat melakukan pendaftaran dengan informasi:

- Nama lengkap
- Email
- Kata sandi
- Nama usaha
- Alamat usaha
- Nomor telepon

Setelah registrasi, akun penyedia memiliki status verifikasi `menunggu`.

Penyedia belum dapat mengelola penawaran sebelum akun disetujui oleh pihak yang berwenang.

### C. Peninjauan dan Verifikasi Penyedia

Verifikasi dilakukan melalui proses peninjauan akun penyedia.

Script verifikasi tersedia melalui perintah:

```bash
npm run penyedia:verifikasi -- email@domain.com
```

Untuk menolak verifikasi penyedia:

```bash
npm run penyedia:verifikasi -- email@domain.com --tolak
```

Proses ini memperbarui status verifikasi penyedia berdasarkan keputusan peninjauan.

Status verifikasi digunakan untuk membatasi akses penyedia terhadap fitur pengelolaan penawaran.

## 8. Menjalankan Aplikasi

Jalankan server dalam mode pengembangan:

```bash
npm run dev
```

Server menggunakan alamat lokal:

```text
http://127.0.0.1:3000
```

Jika halaman frontend disediakan melalui folder `public`, akses halaman melalui browser menggunakan alamat server tersebut dan path halaman yang tersedia pada proyek.

Untuk menghentikan server, tekan:

```text
Ctrl + C
```

**Catatan:** Jalankan server hanya satu kali pada port yang sama. Jika port 3000 sedang digunakan, periksa proses yang berjalan sebelum menjalankan server kembali.

## 9. Pengujian Autentikasi

Pengujian autentikasi Chapter 8 dapat dijalankan menggunakan:

```bash
npm run auth:test
```

Pengujian mencakup beberapa skenario:

- Registrasi akun pembeli.
- Registrasi akun penyedia.
- Login menggunakan akun terdaftar.
- Penolakan login dengan kata sandi yang salah.
- Penolakan registrasi dengan manipulasi peran.
- Pemeriksaan status verifikasi penyedia.
- Pembatasan akses penyedia yang belum terverifikasi.
- Pemeriksaan kepemilikan penawaran.
- Pemeriksaan kepemilikan pesanan.
- Pengujian logout dan validasi token.

Hasil pengujian aktual yang telah diperoleh:

```text
PASS: 44 pemeriksaan HTTP dan assertion kepemilikan/snapshot.
Bukti: penawaran 12, pesanan 6; fixture dipertahankan.
Empat akun uji dibuat dengan sandi acak yang tidak dicetak.
```

Hasil tersebut menunjukkan bahwa 44 pemeriksaan pada script autentikasi berhasil dijalankan.

## 10. Kebijakan Keamanan Autentikasi

Aplikasi menerapkan beberapa mekanisme keamanan:

1. Kata sandi disimpan dalam bentuk hash menggunakan bcryptjs.
2. Token autentikasi menggunakan JWT.
3. Informasi sensitif seperti hash kata sandi tidak dikirimkan dalam respons API.
4. Validasi input dilakukan sebelum data diproses.
5. Akses endpoint tertentu memerlukan autentikasi.
6. Akses pengelolaan penawaran dibatasi berdasarkan status verifikasi penyedia.
7. Pengguna tidak dapat menentukan peran akun melalui manipulasi request registrasi.
8. Pengelolaan data penawaran dan pesanan menerapkan pemeriksaan kepemilikan.

### Kebijakan Logout

Logout dilakukan dengan meningkatkan nilai `versi_token` pada akun pengguna.

Perubahan tersebut menyebabkan seluruh token yang masih menggunakan versi sebelumnya tidak lagi dapat digunakan.

Dengan demikian, logout tidak hanya mengakhiri satu sesi, tetapi juga membatalkan seluruh sesi aktif milik akun tersebut yang menggunakan versi token lama.

Pengguna perlu melakukan login kembali untuk memperoleh token yang valid.

## 11. Dokumentasi API

Dokumentasi endpoint API tersedia pada:

```text
docs/API.md
```

Dokumentasi tersebut mencakup:

- Endpoint autentikasi.
- Registrasi pembeli dan penyedia.
- Login, profil pengguna, dan logout.
- Pengelolaan penawaran.
- Pengelolaan pesanan.
- Validasi request.
- Status respons dan error.
- Kebijakan akses berdasarkan peran dan kepemilikan.

## 12. Dokumentasi Hasil Pengujian

Hasil pengujian Chapter 8 dicatat dalam:

```text
docs/hasil-uji-chapter8.md
```

Dokumen ini digunakan untuk mencatat:

- Commit yang diuji.
- Nama database.
- Fixture yang digunakan.
- Hasil pengujian autentikasi.
- Bukti penolakan manipulasi peran.
- Bukti pembatasan kepemilikan.
- Bukti perubahan versi token.
- Hasil aktual tanpa mencantumkan JWT, hash kata sandi, atau kata sandi asli.

## 13. Git dan Commit

Periksa perubahan file sebelum melakukan commit:

```bash
git status --short
```

Periksa perubahan yang belum masuk staging:

```bash
git diff
```

Periksa perubahan yang sudah masuk staging:

```bash
git diff --cached
```

Tambahkan file dokumentasi yang diperlukan:

```bash
git add README.md docs/API.md docs/hasil-uji-chapter8.md
```

Periksa kembali file yang akan masuk commit:

```bash
git status --short
git diff --cached
```

Lakukan commit setelah memastikan tidak ada file rahasia atau file yang tidak diperlukan:

```bash
git commit -m "docs: update chapter 8 authentication documentation"
```

Periksa commit terakhir:

```bash
git log -1 --oneline
```

## 14. Catatan Pengembangan

SisaBaik dikembangkan sebagai proyek pembelajaran Pengembangan Web dengan fokus pada implementasi backend, pengelolaan database relasional, transaksi, autentikasi, otorisasi, dan pengujian API.

Dokumentasi ini menjadi panduan instalasi, konfigurasi, penggunaan, serta pengujian aplikasi SisaBaik pada Chapter 8.

@'

## 15. Frontend Modul 9 – SisaBaik

Frontend SisaBaik dikembangkan menggunakan Vue.js dan Vite sebagai aplikasi terpisah di dalam folder `frontend/`.

### Persyaratan Sistem

- Node.js versi 22 atau lebih baru.
- npm.
- Browser modern.

### Instalasi dan Menjalankan Frontend

Instal dependensi:

```bash
npm --prefix frontend ci
```
