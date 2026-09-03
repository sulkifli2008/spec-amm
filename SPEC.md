# Website Profile dan Media Pemuda Muhammadiyah — Spec

Sumber: `Website Profile dan Media Pemuda Muhammadiyah v1.docx` (PRD v1.0, 7 Agustus 2026, Author: Ahmad Syauki, Status: Draft)

**Revisi v1.1 — 3 September 2026.** Direvisi berdasarkan pelajaran dari proyek lama (`frontend-pemuda` / `backend-pemuda`, folder `specs/` bergaya spec-kit, fitur 001–009) yang mengimplementasikan versi awal spec serupa dan sebagian besar fiturnya sudah dibangun & diverifikasi. Proyek lama **tidak dijadikan checklist wajib** di sini — hanya dipakai sebagai rujukan keputusan yang sudah teruji di lapangan. Proyek ini (`pemuda-muhammadiyah-portal`) adalah scaffold baru dan terpisah, melanjutkan visi produk yang sama.

Perubahan dari v1.0 ditandai **[v1.1 — dikonfirmasi]** (sudah diputuskan bersama pengguna) atau **[v1.1 — usulan]** (kandidat perubahan dari pengalaman proyek lama, belum diputuskan — lihat "Usulan Tambahan" di bagian akhir). Lihat juga "Riwayat Perubahan" di akhir dokumen untuk ringkasan lengkap.

**Dokumen ini adalah inti/kesimpulan utuh** dari seluruh spec — ringkas dan menaut ke file detail per area fitur (masing-masing berisi konten lengkap, siap dipakai sebagai acuan saat build/SDD):

| File | Isi |
|---|---|
| `AUTH.md` | Autentikasi, Google+Facebook OAuth, penautan akun |
| `ORGANISASI.md` | Organisasi, Struktur Organisasi, Tahun Angkatan, aset logo |
| `ARTIKEL.md` | Artikel, workflow editorial, struktur data, analitik |
| `KOMENTAR.md` | Komentar (termasuk Guest), filter kata kasar, moderasi |
| `LANDING-PAGE.md` | Landing page, akses edit, grid organisasi, dashboard |
| `SCROLL-PAGES.md` | Splash/Intro Screen (scroll parallax) |

## Ringkasan

Portal berita & profil organisasi Pemuda Muhammadiyah dengan sistem editorial berjenjang: organisasi cabang menulis artikel → masuk antrian review → Super Admin approve/reject → baru tayang ke publik. Empat peran (Super Admin, Organisasi, User, Guest) dengan hak akses berbeda. Stack: React 19 + Flask, arsitektur dipisah frontend/backend berbasis fitur/modul, keamanan standar (JWT, RBAC, bcrypt, rate limiting). Intinya: **content management system dengan approval workflow**, bukan company profile statis.

**[v1.1 — dikonfirmasi]** Akun Organisasi dibuat manual oleh Super Admin (bukan permohonan publik yang di-approve), dan Guest kini bisa ikut berkomentar (bukan cuma baca-saja) — lihat detail di §5 dan §8.

## 1. Latar Belakang

Info kegiatan Pemuda Muhammadiyah saat ini tersebar di berbagai media (medsos, grup chat, website cabang masing-masing) → sulit ditemukan, arsip tidak terpusat, kualitas publikasi tidak seragam, tidak ada mekanisme validasi sebelum publikasi.

## 2. Visi Produk

Portal resmi yang menjadi pusat informasi organisasi, media publikasi berita dari seluruh cabang, dengan sistem editorial terstruktur untuk menjaga kualitas dan validitas informasi.

## 3. Tujuan Produk

- Website resmi Pemuda Muhammadiyah
- Media publikasi berita seluruh organisasi cabang, tersatukan dalam satu portal
- Mempermudah masyarakat memperoleh informasi resmi
- Sistem verifikasi berita sebelum dipublikasikan
- Analisis performa artikel bagi organisasi
- Keterlibatan masyarakat melalui komentar dan reaksi

## 4. Problem Statement

- Publikasi berita tersebar di berbagai platform, tanpa standar kualitas
- Tidak ada mekanisme verifikasi sebelum publish
- Sulit cari arsip berita & mengetahui performa artikel
- Belum ada portal resmi yang mewakili organisasi

## 5. Target Pengguna & Hak Akses

**Super Admin** — kelola landing page (`LANDING-PAGE.md`), organisasi (**[v1.1] buat akun manual**, `ORGANISASI.md`), banner, kategori, tag; verifikasi/approve/reject artikel (`ARTIKEL.md`); moderasi komentar (**[v1.1]**, `KOMENTAR.md`); lihat statistik sistem.

**Organisasi** (akun cabang) — login dashboard; CRUD artikel & workflow editorial (`ARTIKEL.md`); kelola profil & Struktur Organisasi sendiri (`ORGANISASI.md`); balas komentar; lihat statistik & analitik. **[v1.1]** Akun dibuat oleh Super Admin, bukan mengajukan permohonan sendiri.

**User** (login) — baca artikel; like/unlike; komentar + balas komentar; edit profil. **[v1.1]** Login via Google **atau Facebook OAuth**, dengan kebijakan penautan akun berbasis email terverifikasi — detail lengkap di `AUTH.md`.

**Guest** — baca landing page & artikel; cari artikel. **[v1.1] Boleh ikut berkomentar** dengan nama otomatis per-perangkat, detail di `KOMENTAR.md` §1. **Guest TIDAK LAGI bisa mengajukan permohonan akun organisasi** — lihat `ORGANISASI.md` §1.

## 6. User Story (contoh per role)

- Super Admin: memverifikasi artikel agar hanya yang layak dipublikasikan; membuat & mengelola akun organisasi agar hanya organisasi resmi yang menerbitkan berita; menindaklanjuti komentar yang dilaporkan agar diskusi publik tetap sehat.
- Organisasi: membuat artikel agar kegiatan bisa dipublikasikan; melihat statistik untuk tahu performa.
- User: memberi komentar untuk diskusi; memberi like untuk apresiasi.
- Guest: membaca berita untuk info terbaru; ikut berkomentar tanpa perlu bikin akun dulu, sebagai jalan masuk sebelum akhirnya mendaftar.

## 7. Ruang Lingkup Sistem

Sistem terdiri dari 5 area utama. Tiap area dirinci di file `.md` mandiri (isi lengkap, siap dipakai sebagai acuan saat build/SDD) — bagian ini hanya ringkasan + pointer supaya SPEC.md tetap jadi kesimpulan utuh yang ringkas dibaca:

- **Splash/Intro Screen** *(baru, [v1.1])* — layar pembuka terpisah sebelum landing page, satu scroll menyambung dua bagian (pengenalan organisasi + Visi Misi, lanjut ke Sejarah gerakan secara umum), tampil sekali per pengunjung, diakhiri tombol "Lanjutkan". → **`SCROLL-PAGES.md`**
- **Landing Page & Dashboard** — topik landing page, prinsip akses edit (dikunci kode vs. panel edit), grid pemilih organisasi (hardcode), menu Dashboard Organisasi & Super Admin. → **`LANDING-PAGE.md`**
- **Organisasi & Struktur Organisasi** — pembuatan akun manual oleh Super Admin (**[v1.1]**), dua-jalur pengelolaan konten per-organisasi, bagan kepengurusan + Tahun Angkatan + akun per anggota, aset logo 8 ortonom. → **`ORGANISASI.md`**
- **Artikel & Alur Editorial** — CRUD, workflow status, verifikasi, struktur data & tampilan, analitik. → **`ARTIKEL.md`**
- **Komentar & Moderasi** — komentar Guest bernama otomatis (**[v1.1]**), filter kata kasar, laporan & moderasi Super Admin. → **`KOMENTAR.md`**

**Autentikasi & Penautan Akun** (Google + Facebook OAuth, auto-link berdasar email terverifikasi, **[v1.1]**) dirinci di **`AUTH.md`** — lihat juga §5 di atas.

## 8. Functional Requirements (ringkasan — FR-001 s/d FR-040 asli)

Daftar lengkap tiap kategori FR ada di file masing-masing (§7). Ringkasan kategori:

| Kategori | File detail |
|---|---|
| Authentication, Reset/Ganti Password, Penautan Akun (Google+Facebook, **[v1.1]**) | `AUTH.md` |
| Organisasi (CRUD manual oleh Super Admin, **[v1.1]**), Struktur Organisasi (Tahun Angkatan, dsb.) | `ORGANISASI.md` |
| Artikel (CRUD, workflow, verifikasi), Struktur & Detail Artikel, Analitik | `ARTIKEL.md` |
| Komentar (termasuk Guest, **[v1.1]**), Filter kata kasar, Moderasi Komentar (**[v1.1]**) | `KOMENTAR.md` |
| Landing Page, Dashboard | `LANDING-PAGE.md` |
| Splash/Intro (**[v1.1]**) | `SCROLL-PAGES.md` |

## 9. Workflow Artikel, Status, Struktur Data & Analitik

Dipindah ke **`ARTIKEL.md`** (§2–6): diagram workflow (Draft → Pending Review → Approve/Reject → Published), daftar status, struktur data artikel, detail tampilan, dan analitik dashboard organisasi.

## 10. Non-Functional Requirements

Responsive Design, HTTPS, JWT Authentication, OAuth Google **& Facebook** ([v1.1]), PostgreSQL, Cloud Storage, Audit Log, RBAC, Response Time < 500ms, Backup Database Harian, Upload Gambar Maks 5MB + Kompresi Otomatis, SEO Friendly (XML Sitemap, Robots.txt, Open Graph Metadata), Proteksi SQL Injection/XSS/CSRF, Rate Limiting (login & komentar), **[v1.1] Filter kata kasar pada komentar**.

## 11. Out of Scope (v1.0)

Mobile Application, Multi Bahasa, Live Streaming, Chat Antar Pengguna, Push Notification Mobile, Integrasi WhatsApp, Integrasi AI untuk penulisan artikel, **[v1.1] pendaftaran/onboarding akun organisasi secara publik/self-service** (digantikan pembuatan akun manual oleh Super Admin, lihat §5/§8).

## 12. Success Metrics

- Seluruh organisasi publikasi via satu portal
- 100% artikel melalui proses verifikasi
- Landing page jadi media resmi profil organisasi
- Publikasi lebih cepat dibanding proses manual
- Portal tahan naik jumlah artikel/pengguna tanpa penurunan performa signifikan
- Pengunjung aktif & interaksi meningkat bertahap

## 13. Roadmap

- **MVP (v1.0)**: Landing Page, Login, Dashboard Organisasi, Dashboard Super Admin, CRUD Artikel, Verifikasi Artikel, Publish Artikel, Komentar (User **dan Guest**, **[v1.1]**), Moderasi Komentar (**[v1.1]**), Like & Unlike, **[v1.1] pembuatan akun organisasi manual oleh Super Admin** (menggantikan Request Akun Organisasi).
- **v1.1**: Analitik Dashboard, Media Library, Banner Management, SEO Management, Notifikasi Email, Pencarian dan Filter.
- **v2.0**: PWA, Multi Bahasa, Bookmark Artikel, Pelaporan Komentar (⚠️ sudah masuk MVP di revisi ini sebagai Moderasi Komentar — item ini disisakan dari PRD asli untuk konsistensi historis, pertimbangkan dihapus), Integrasi Media Sosial, Rekomendasi Artikel, Integrasi AI untuk penyusunan artikel.

## 14. Technical Architecture

### 14.1 Tujuan Arsitektur
Scalable, Maintainable, Secure, High Performance, Responsif, mendukung pengembangan jangka panjang, pemisahan tanggung jawab frontend/backend, API First.

### 14.2 Technology Stack

**Frontend**: React 19, Vite, Redux Toolkit + RTK Query, React Router DOM, Tailwind CSS (+ CSS Modules opsional), Anime.js, React Hook Form + Zod, Lucide React, Tiptap Editor, Recharts, react-lazy-load-image-component, React Hot Toast, Day.js, clsx. **[v1.1]** Tambahan khusus untuk Splash/Intro Screen & Sejarah scroll-reveal (lihat `SCROLL-PAGES.md` §3): **Framer Motion**, **Lenis**, **react-icons**. Anime.js tetap dipertahankan untuk animasi lain sesuai §14.7 — bukan diganti.

**Backend**: Flask, SQLAlchemy, Alembic + Flask-Migrate, Flask-JWT-Extended, Marshmallow, python-dotenv, Flask-Upload + Pillow, Gunicorn

**Database**: PostgreSQL
**Storage**: Local (dev) / Cloudinary (prod), alternatif Amazon S3 / Supabase Storage
**API Docs**: OpenAPI 3.1 + Swagger UI
**VCS**: Git + GitHub
**Deployment**: Frontend (Vite Build + Nginx + PM2), Backend (Flask + Gunicorn + Nginx), Ubuntu Server

### 14.3 Arsitektur Sistem

```
                        Client Browser
                              │
              ┌───────────────┴───────────────┐
              ▼                               ▼
      Landing Page                       Dashboard
      React + Vite                      React + Redux
              │                               │
              └───────────────┬───────────────┘
                               ▼
                       RESTful API
                    Flask + SQLAlchemy
                               │
          ┌────────────────────┴────────────────────┐
          ▼                                          ▼
     PostgreSQL                              Cloud Storage
```

### 14.4 Arsitektur Frontend (Feature Based)

```
src/
├── app/
├── assets/
├── components/
├── layouts/
├── pages/
├── features/
│   ├── auth/
│   ├── organization/
│   ├── article/
│   ├── comments/
│   ├── dashboard/
│   └── landing/
├── services/
├── hooks/
├── routes/
├── store/
├── utils/
└── constants/
```

### 14.5 Arsitektur Backend (Modular Flask)

```
backend/
app/
├── auth/
├── organization/
├── article/
├── comments/
├── landing/
├── dashboard/
├── users/
├── uploads/
├── middleware/
├── services/
├── repositories/
├── models/
├── schemas/
├── utils/
├── config/
└── extensions/
```

### 14.6 Prinsip UI/UX

Clean Interface, Minimalist Layout, Mobile First, Responsive Design, Fast Loading, Accessible, Konsisten di seluruh halaman, mudah dipelajari tanpa pelatihan.

### 14.7 Animasi (Anime.js)

Dipakai untuk enhance UX, tidak mengganggu baca artikel/dashboard. Diterapkan di: Landing Page (hero, fade in, reveal, scroll, counter, hover, floating), Dashboard (sidebar transition, modal, loading skeleton, card hover, notification, page transition), Artikel (smooth scroll, lazy image reveal, like/comment animation, micro interaction, ripple, success/error animation).

Prinsip: durasi maks 300ms, tidak ganggu fokus, respect `prefers-reduced-motion`, gunakan `transform`/`opacity` untuk GPU acceleration.

### 14.8 Keamanan

JWT Authentication, RBAC, Password Hashing (bcrypt), HTTPS Only, proteksi SQL Injection/XSS/CSRF, Rate Limiting, Input Validation, Audit Log, Secure Cookie, Refresh Token.

### 14.9 Standar API (REST)

```json
// Success
{ "success": true, "message": "Success", "data": {} }

// Validation Error
{ "success": false, "message": "Validation Error", "errors": {} }

// Unauthorized
{ "success": false, "message": "Unauthorized" }
```

### 14.10 Standar Kode

**Backend**: PEP 8, Type Hint, Service Layer, Modular Architecture, Repository Pattern (opsional)
**Frontend**: Functional Component, Custom Hook, Reusable Component, Atomic Design, Lazy Loading, Code Splitting

### 14.11 Target Performa

FCP < 2s, LCP < 2.5s, API Response < 500ms, Lighthouse Score ≥ 90, Lazy Loading Image, Infinite Scroll (daftar artikel), Pagination (dashboard), optimasi gambar otomatis, Code Splitting via Vite, asset minification saat build.

### 14.12 Testing

**Backend**: Unit, Integration, API Testing (Pytest)
**Frontend**: Component, UI, E2E opsional (Vitest, React Testing Library)
**Tools tambahan**: Postman

### 14.13 Monitoring & Logging

Audit Log aktivitas pengguna, Error Logging backend, Request Logging, Monitoring storage/performa API/login gagal/aktivitas organisasi.

### 14.14 Skalabilitas (arah pengembangan masa depan)

Mobile Application, PWA, Integrasi AI (penyusunan & rekomendasi artikel), Multi Bahasa, Multi Wilayah, Multi Organisasi, REST API Versioning, GraphQL Gateway, Notification Service, Search Engine (Elasticsearch/Meilisearch), CDN, Message Queue (Redis/RabbitMQ) untuk proses async (email, notifikasi).

## Usulan Tambahan (dari pengalaman proyek lama, BELUM diputuskan)

Bagian ini murni kandidat — dikumpulkan dari fitur yang benar-benar dibangun & diuji di proyek lama (`frontend-pemuda`), tapi **tidak diminta konfirmasinya** di sesi ini. Jangan dianggap bagian aktif dari spec sampai didiskusikan dan dipindah ke bagian utama.

- **Agenda sebagai tampilan kalender bulanan** (bukan daftar list) — entri Agenda/rapat tampil biru, entri Program Kerja yang sudah dijadwalkan tampil merah pada tanggal yang sama.
- **Login terpadu satu halaman** untuk Organisasi/User/Super Admin, plus **pendaftaran User via email+password** sebagai pelengkap login Google (PRD awal hanya menyebut Google OAuth untuk User; di praktiknya dibutuhkan juga jalur email+password supaya User tidak wajib punya akun Google).
- **Notifikasi in-app** (selain Notifikasi Email yang sudah ada di roadmap v1.1) — setiap pengguna login melihat daftar notifikasi miliknya dan menandainya sudah dibaca.
- **Atribusi verifikator pada artikel**: nama Super Admin yang meng-approve ditampilkan di artikel published (sebagai "diverifikasi oleh"), terpisah dari nama penulis asli.
- **Artikel dengan galeri foto (multi-foto/slideshow)**, bukan cuma satu thumbnail, dan **flag Prioritas** yang bisa diatur Super Admin dari halaman manajemen artikel untuk menonjolkan artikel tertentu.
- **Halaman Super Admin "semua artikel lintas organisasi"** (terpisah dari antrian verifikasi yang hanya berisi status Pending Review) — untuk melihat & mengedit artikel dari organisasi manapun dalam satu tempat.
- **Media Library tanpa tabel database baru**: proyek lama membangunnya dengan memindai folder upload + kolom URL yang sudah dirujuk model lain, bukan mencatat setiap unggahan ke tabel terpisah — pendekatan murah yang terbukti cukup untuk skala ratusan file; relevan sebagai referensi teknis saat FR Media Library ini dikerjakan.

## Asumsi

- Backend REST API mengikuti kontrak yang akan didefinisikan terpisah (mis. OpenAPI); frontend tidak perlu menunggu perubahan kontrak untuk memulai implementasi bagian manapun yang endpoint-nya sudah terdefinisi.
- Autentikasi menggunakan access token JWT di memory/state dan refresh token via HttpOnly cookie.
- "Out of Scope Versi 1.0" mengikuti PRD §11 (lihat §11 di atas untuk daftar terkini termasuk revisi v1.1).

## Riwayat Perubahan

**v1.1 (3 September 2026)** — Revisi berdasarkan pelajaran dari proyek lama `frontend-pemuda`/`backend-pemuda` (bukan sebagai checklist wajib, murni referensi keputusan yang sudah teruji):

1. **[dikonfirmasi]** Registrasi organisasi: dari "Guest mengajukan permohonan, Super Admin approve" menjadi "Super Admin membuat akun organisasi secara manual". Tidak ada lagi form publik.
2. **[dikonfirmasi]** Komentar Guest: dari "Guest read-only, hanya User login yang bisa komentar/like" menjadi "Guest boleh berkomentar dengan nama otomatis per-perangkat (berganti ke nama akun saat login)", disertai filter kata kasar untuk semua pengirim dan Moderasi Komentar oleh Super Admin.
3. **[dikonfirmasi]** Scope Landing Page CMS: dipersempit — konten identitas resmi (Hero, navbar, Sejarah, Visi & Misi) dikunci ke kode, tidak diedit lewat panel; yang punya panel edit hanya FAQ (Super Admin) dan Struktur Organisasi/Program Kerja/Agenda/Galeri per organisasi (dua jalur: organisasi ybs. + Super Admin). **Susulan (3 September 2026)**: tombol/grid pemilih organisasi di landing page (daftar organisasi yang muncul & urutannya, mis. grid Muhammadiyah/Aisyiyah/Pemuda Muhammadiyah/dst) juga masuk kategori "dikunci ke kode" ini — bukan diatur lewat panel atau database.
4. **[dikonfirmasi]** Fitur KTAM (Kartu Tanda Anggota Muhammadiyah) yang muncul di proyek lama **tidak dimasukkan** ke spec ini — dianggap di luar cakupan produk sepenuhnya, tidak disebut lebih lanjut.
5. **[dikonfirmasi, 3 September 2026]** Tombol/grid pemilih organisasi di landing page ikut masuk kategori "dikunci ke kode" (§7).
6. **[dikonfirmasi, 3 September 2026]** Struktur Organisasi diperkaya mengikuti bentuk proyek lama — bagan kepengurusan hierarkis + foto, konsep **Tahun Angkatan** (periode kepengurusan per rentang tahun, bisa dipilih untuk lihat histori), akun login per anggota struktur, kartu profil hover/tap + statistik, field privat nomor kontak/KTM, dan autocomplete nama Bidang. Detail lengkap di §7 "Struktur Organisasi (detail)".
7. **[usulan, belum diputuskan]** Kandidat pengayaan lain dari proyek lama masih dikumpulkan di bagian "Usulan Tambahan" di atas: Agenda sebagai kalender, login terpadu + signup User email/password, notifikasi in-app, atribusi verifikator pada artikel, galeri foto artikel + flag prioritas, halaman artikel lintas organisasi untuk Super Admin, dan pendekatan teknis Media Library tanpa tabel baru.
8. **[dikonfirmasi, 3 September 2026]** Ditambahkan **Splash/Intro Screen**: layar pembuka terpisah sebelum landing page, scroll-parallax berisi pengenalan organisasi + Visi & Misi, tombol "Lanjutkan" menuju landing page utama — mengadaptasi pola komponen referensi `SmoothScrollHero` yang dikirim pengguna (Lenis smooth-scroll + Framer Motion parallax, bukan space launch schedule aslinya). Library Framer Motion, Lenis, react-icons ditambahkan ke stack frontend khusus untuk layar ini; Anime.js tetap dipakai di tempat lain, tidak diganti.
9. **[dikonfirmasi, 3 September 2026]** Aset logo 8 ortonom (SVG) diterima lengkap dan disimpan di `assets/logos/` folder proyek — lihat §7 "Struktur Organisasi (detail)".
10. **[dikonfirmasi, 3 September 2026]** Topik "Sejarah" per organisasi ditampilkan sebagai scroll-reveal milestone (judul + scroll indicator → babak-babak sejarah berselang-seling kiri-kanan dengan efek clip-path reveal + fade saat discroll → layar penutup) — mengadaptasi pola komponen referensi `parallax-scroll-feature-section`, memakai ulang Framer Motion & Lucide React yang sudah ada di stack, tanpa dependency baru. Konten tiap milestone tetap hardcode per organisasi, konsisten dengan keputusan Sejarah terkunci ke kode.
11. **[dikonfirmasi, 3 September 2026]** Isi foto di Splash/Intro Screen ditentukan: gambar tengah *sticky* = logo Muhammadiyah, 4 slot mosaic parallax = logo Pemuda Muhammadiyah, Aisyiyah, Nasyiatul Aisyiyah, IPM (urutan sesuai anotasi visual pengguna). Sengaja hanya 5 dari 8 organisasi tampil di splash; Tapak Suci/Hizbul Wathan/IMM cukup di grid pemilih organisasi landing page.
12. **[dikonfirmasi, 3 September 2026]** Login/registrasi User ditambah **Facebook OAuth** di samping Google (PRD awal hanya Google). Kebijakan penautan akun: **auto-link berdasar email terverifikasi** — Google, Facebook, dan email/password disatukan otomatis kalau emailnya cocok dan terverifikasi provider, tanpa layar konfirmasi tambahan; kalau tidak terverifikasi, auto-link ditolak dan pengguna diarahkan menghubungkan manual dari halaman Profil setelah login lewat email/password. Tidak ada "perjanjian" khusus yang dibutuhkan antara Google dan Facebook — keduanya provider OAuth independen, kebijakan penautan murni ada di sisi aplikasi ini.
13. **[dikonfirmasi, 3 September 2026]** Detail Splash/Intro Screen **dipindah ke file terpisah `SCROLL-PAGES.md`** supaya SPEC.md tidak kepanjangan — SPEC.md sekarang hanya menyimpan ringkasan + pointer, isi lengkap (perilaku, pemetaan logo, dependency, kode referensi) ada di file baru itu.
14. **[dikonfirmasi, koreksi 3 September 2026]** Dua komponen referensi yang dikirim pengguna (`SmoothScrollHero` dan `parallax-scroll-feature-section`) ternyata dimaksudkan sebagai **satu scroll yang menyambung**, bukan dua fitur terpisah seperti tertulis sebelumnya. Akibatnya: **"Sejarah" bukan topik terpisah per organisasi** — dihapus dari daftar topik dua-jalur Tentang Kami dan dari daftar konten terkunci per-organisasi; Sejarah kini murni Bagian 2 dari Splash/Intro Screen, menceritakan sejarah gerakan Pemuda Muhammadiyah secara umum (bukan sejarah tiap ortonom satu-satu). Topik per-organisasi yang tersisa: Visi & Misi (dikunci kode), Struktur Organisasi/Program Kerja/Agenda/Galeri (dua-jalur, panel edit).
15. **[dikonfirmasi, 3 September 2026]** Splash/Intro Screen dipastikan **hanya tampil satu kali** — kunjungan pertama saja; kunjungan berikutnya langsung masuk Beranda tanpa Splash (sebelumnya masih berstatus usulan default).
16. **[dikonfirmasi, 3 September 2026]** SPEC.md dirombak jadi **dokumen inti/kesimpulan utuh** yang ringkas — detail penuh tiap area fitur dipecah ke file `.md` mandiri per fitur, mengikuti pola yang sama dengan `SCROLL-PAGES.md`: **`AUTH.md`** (autentikasi & penautan akun), **`ORGANISASI.md`** (organisasi & struktur organisasi), **`ARTIKEL.md`** (artikel & alur editorial), **`KOMENTAR.md`** (komentar & moderasi), **`LANDING-PAGE.md`** (landing page & dashboard). §5, §7, §8, §9 di SPEC.md kini hanya ringkasan + pointer ke file-file itu; nomor bagian §10–§14 (Non-Functional s/d Technical Architecture) ikut bergeser dari §14–§18 sebelumnya. Tujuannya supaya tiap file cukup detail dipakai sebagai acuan langsung saat build/SDD nanti, sementara SPEC.md sendiri tetap enak dibaca sebagai gambaran utuh.
17. **[dikonfirmasi, 3 September 2026]** Sinkronisasi `LANDING-PAGE.md` dengan file detail lain (`ARTIKEL.md`, `ORGANISASI.md`, `KOMENTAR.md`, `AUTH.md`, `SCROLL-PAGES.md`): daftar tab status Artikel di menu Dashboard Organisasi diselaraskan dengan daftar lengkap 7 status di `ARTIKEL.md` §3 (sebelumnya hanya mencantumkan 5 dari 7); daftar FR Landing Page diselaraskan dengan daftar topik §1 (`LANDING-PAGE.md`); ditambahkan catatan fase roadmap untuk Media Library & Banner di menu Dashboard Super Admin (masuk v1.1, bukan MVP — lihat §13); ditambahkan catatan alur masuk pengunjung baru lewat Splash/Intro Screen sebelum landing page (`SCROLL-PAGES.md`). Murni penyelarasan referensi antar-file, tidak ada perubahan keputusan produk baru.

## Catatan

Ada project lama di `c:\Users\dixk\pemuda\` (folder `backend-pemuda` dan `frontend-pemuda`) yang mengimplementasikan spec serupa — sebagian besar fiturnya (spec 002–008 di `frontend-pemuda/specs/`) sudah dibangun & diverifikasi, dan menjadi rujukan revisi v1.1 di atas. Folder ini (`pemuda-muhammadiyah-portal`) tetap dibuat sebagai project baru yang terpisah — belum di-scaffold, baru berisi spec ini sebagai acuan awal.
