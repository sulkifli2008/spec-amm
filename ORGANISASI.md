# Organisasi & Struktur Organisasi — Detail

Dokumen pelengkap `SPEC.md` (§5, §7, §8) — detail lengkap pengelolaan akun Organisasi dan konten Struktur Organisasi, dipisah ke file ini supaya SPEC.md tetap ringkas sebagai ringkasan/kesimpulan utuh. Status semua bagian di bawah: **[v1.1 — dikonfirmasi]**, kecuali ditandai lain.

## 1. Pembuatan & Pengelolaan Akun Organisasi

**[v1.1 — dikonfirmasi, koreksi dari PRD awal]** Akun Organisasi (akun cabang) **dibuat manual oleh Super Admin** — bukan lewat permohonan publik yang di-approve seperti di PRD v1.0. Tidak ada lagi form "Daftar Organisasi"/"Request Akun Organisasi" untuk Guest; tidak ada alur onboarding organisasi yang bersifat publik/self-service di v1 ini.

Kewenangan Super Admin atas akun Organisasi:

- **Buat akun organisasi secara manual** (CRUD).
- **Aktivasi / nonaktifkan** akun.
- **Reset password** organisasi.

Kewenangan Organisasi (akun cabang) begitu login:

- Dashboard sendiri; CRUD artikel; simpan draft; submit review; publish (setelah disetujui) / unpublish.
- Kelola profil organisasi sendiri.
- Balas komentar pada artikelnya.
- Lihat statistik artikel & analitik pembaca (lihat `ARTIKEL.md` §5 Analitik).

## 2. Dua Jalur Pengelolaan Konten Per-Organisasi

Sebagian konten "Tentang Kami" per organisasi dikelola lewat **dua jalur yang berlaku seragam**: organisasi yang login mengelola datanya sendiri, dan Super Admin dapat mengelola organisasi manapun (termasuk yang belum/tidak punya akun aktif) — pola ini dipakai supaya organisasi tanpa akun pun tetap bisa "diperkenalkan" di portal.

Topik yang punya dua-jalur ini: **Struktur Organisasi, Program Kerja, Agenda, Galeri**.

Topik yang justru **dikunci ke kode** (tidak ada panel edit sama sekali, demi keamanan identitas — lihat `LANDING-PAGE.md` §2 untuk penjelasan lengkap prinsipnya): **Visi & Misi per organisasi**. (Sejarah TIDAK termasuk topik per-organisasi lagi sejak koreksi v1.1 — lihat `SCROLL-PAGES.md`, Sejarah kini hanya bagian dari Splash/Intro Screen, tentang gerakan secara umum.)

## 3. Struktur Organisasi (Detail)

Konten topik "Struktur Organisasi" per organisasi (salah satu topik dua-jalur di §2) mengikuti bentuk yang sudah teruji di proyek lama (`frontend-pemuda`):

- **Bagan kepengurusan hierarkis**: Ketua Umum di puncak → turun ke Sekretaris Umum & Bendahara Umum → per Bidang: Ketua Bidang → Sekretaris Bidang → Anggota. Tiap orang di bagan tampil dengan foto.
- **Tahun Angkatan** (disebut juga Periode Kepengurusan): setiap susunan struktur terikat ke satu rentang tahun (mis. "2023–2025"). Organisasi/Super Admin dapat menambah Tahun Angkatan baru kapan pun; struktur Tahun Angkatan lama tetap tersimpan sebagai arsip, tidak ditimpa. Pengunjung publik secara default melihat Tahun Angkatan yang aktif (mencakup tahun berjalan, atau yang paling baru bila tidak ada yang mencakup tahun berjalan), dan dapat memilih Tahun Angkatan lain untuk melihat histori kepengurusan tanpa pindah halaman.
- **Akun login per anggota struktur**: tiap anggota yang dimasukkan ke bagan punya akun login sendiri untuk mengedit profil miliknya sendiri (bio, foto, kontak, tautan media sosial — ikon platform terdeteksi otomatis dari URL yang diisi, mis. Facebook/Instagram/Twitter/GitHub/TikTok).
- **Kartu profil saat hover/tap**: menyentuh/mengarahkan kursor ke satu orang di bagan menampilkan panel profil singkat (foto, nama, bidang, bio, kontak, medsos) beserta statistik like/komentar/share dari artikel yang ia tulis.
- **Field privat**: nomor kontak dan nomor KTM/KTA (kartu tanda anggota) diisi per anggota tapi TIDAK tampil di profil publik secara default — hanya terlihat oleh organisasi pemilik dan Super Admin saat mengelola.
- **Autocomplete nama Bidang**: nama Bidang yang sudah pernah diinput di suatu organisasi disarankan otomatis saat menambah anggota struktur baru, supaya tidak perlu diketik ulang tiap kali.

## 4. Aset Logo 8 Ortonom

**[v1.1 — dikonfirmasi]** Aset logo 8 ortonom sudah tersedia, disimpan di `assets/logos/` folder proyek ini (format SVG, vektor — lebih baik dari rencana PNG 1024×1024 di proyek lama):

`Logo-Muhammadiyah.svg`, `Logo-Aisyiyah.svg`, `Logo-PM.svg` (Pemuda Muhammadiyah), `Logo-NA.svg` (Nasyiatul Aisyiyah), `Logo-IMM.svg`, `Logo-IPM.svg`, `Logo-Tapak-Suci.svg`, `Logo-HW.svg` (Hizbul Wathan) — lengkap 8/8, dipakai di grid pemilih organisasi landing page (`LANDING-PAGE.md` §3) dan 5 di antaranya (Muhammadiyah, PM, Aisyiyah, NA, IPM) juga dipakai di Splash/Intro Screen (`SCROLL-PAGES.md`).

## 5. Functional Requirements

- **Organisasi**: CRUD (dibuat manual oleh Super Admin, bukan aktivasi dari permohonan publik), Aktivasi, Suspend, Reset Password.
