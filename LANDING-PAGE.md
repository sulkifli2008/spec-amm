# Landing Page & Dashboard — Detail

Dokumen pelengkap `SPEC.md` (§7) — detail lengkap ruang lingkup Landing Page, prinsip akses edit, grid pemilih organisasi, dan menu dashboard, dipisah ke file ini supaya SPEC.md tetap ringkas sebagai ringkasan/kesimpulan utuh. Status semua bagian di bawah: **[v1.1 — dikonfirmasi]**, kecuali ditandai lain.

## 1. Topik Landing Page

Hero, Tentang Organisasi, Visi Misi, Struktur Organisasi, Program Kerja, Agenda, Galeri, Berita Terbaru, Kontak, FAQ.

**[v1.1 — dikonfirmasi]** "Request Akun Organisasi" **dihapus** dari landing page — tidak ada lagi form permohonan publik (lihat `ORGANISASI.md` §1, `AUTH.md` §4).

*(Catatan: "Sejarah" bukan lagi topik landing page tersendiri sejak koreksi v1.1 — kini hanya bagian dari Splash/Intro Screen, lihat `SCROLL-PAGES.md`.)*

*(Catatan: pengunjung yang membuka situs untuk pertama kali melewati Splash/Intro Screen dulu sebelum sampai ke landing page ini — lihat `SCROLL-PAGES.md` §1. Kunjungan berikutnya langsung ke landing page ini tanpa Splash — lihat `SCROLL-PAGES.md` "Perilaku tampil".)*

## 2. Prinsip Pembagian Akses Edit

Menggantikan asumsi PRD awal bahwa semua konten landing page bisa diedit lewat CMS Super Admin secara merata:

- **Dikunci ke kode/deploy** (tidak diedit lewat panel admin mana pun): Hero/banner utama, menu navigasi utama, Visi & Misi per organisasi. Alasannya keamanan — konten ini berfungsi sebagai **identitas resmi**, jadi tidak boleh ada risiko diubah sembarangan lewat kompromi akun admin, hanya bisa berubah lewat rilis kode baru.
- **Punya panel edit** (dua jalur: organisasi ybs. untuk datanya sendiri, dan Super Admin untuk organisasi manapun): **Struktur Organisasi, Program Kerja, Agenda, Galeri** per organisasi — lihat `ORGANISASI.md` §2–3.
- **Punya panel edit** (Super Admin saja): **FAQ**.

## 3. Grid Pemilih Organisasi (Hardcode)

**[v1.1 — dikonfirmasi]** Landing page (khususnya halaman "Tentang Kami") menampilkan tombol/kartu untuk memilih organisasi mana yang datanya ingin dilihat: Muhammadiyah, Aisyiyah, Pemuda Muhammadiyah, Nasyiatul Aisyiyah, IMM, IPM, Tapak Suci, Hizbul Wathan (8 slot).

- **Daftar organisasi apa saja yang muncul di pemilih ini beserta urutannya di-hardcode di kode frontend**, bukan diatur lewat panel Super Admin atau tabel database — konsisten dengan prinsip keamanan di §2: siapa saja organisasi resmi yang tampil tidak boleh berubah lewat kompromi akun admin, hanya lewat rilis kode baru.
- Data yang ditampilkan di tiap slot (logo, nama, dan konten profil organisasi ybs.) tetap diambil langsung dari data organisasi terkait, bukan ikut di-hardcode — hanya **susunan slot-nya** yang tetap.
- Aset logo 8 ortonom (SVG) sudah tersedia — lihat `ORGANISASI.md` §4.

## 4. Dashboard Organisasi (menu)

Dashboard, Artikel (tab per status — daftar status lengkap ada di `ARTIKEL.md` §3: Draft/Pending Review/Approved/Rejected/Published/Unpublished/Archived), Komentar, Statistik, Profil Organisasi.

## 5. Dashboard Super Admin (menu)

Dashboard, Landing Page CMS (FAQ + konten per-organisasi, lihat §2), Verifikasi Artikel, Organisasi (termasuk buat akun baru manual — lihat `ORGANISASI.md` §1), Media Library, Banner, Kategori, Tag, User Management, **Moderasi Komentar** (antrian laporan komentar — lihat `KOMENTAR.md` §4), Pengaturan.

*(Catatan: Media Library dan Banner Management masuk roadmap **v1.1** — bukan MVP v1.0, lihat `SPEC.md` §13 Roadmap. Menu ini tetap dicantumkan di sini sebagai gambaran menu Dashboard Super Admin yang utuh; implementasinya menyusul setelah MVP. Kategori & Tag tetap MVP karena sudah jadi field wajib struktur data Artikel — lihat `ARTIKEL.md` §4.)*

## 6. Functional Requirements

- **Landing Page**: Tampilkan seluruh topik di §1 (Hero, Tentang Organisasi, Visi Misi, Struktur Organisasi, Program Kerja, Agenda, Galeri, Berita Terbaru, Kontak, FAQ). Bagian yang dikunci ke kode vs. bagian yang punya panel edit — lihat §2.
- **Dashboard**: Statistik Artikel/Pembaca/Like/Komentar, Artikel Populer — detail lengkap analitik lihat `ARTIKEL.md` §6.
