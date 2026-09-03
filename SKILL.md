# Skill: Mengelola Spec Proyek Ini

Petunjuk ini untuk **siapa pun (AI atau manusia) yang membaca/mengedit spec di folder ini** — bukan cuma sesi yang menulisnya pertama kali. Baca file ini sebelum menambah atau mengubah isi spec proyek `pemuda-muhammadiyah-portal`.

## Aturan Wajib

**Ketika Anda menambahkan spec baru — dengan cara membaca project spec ini (SPEC.md dan file pendukungnya) sebagai konteks — Anda WAJIB membuat file `.md` tersendiri untuk fitur tersebut.** Jangan menumpuk detail fitur baru langsung di dalam `SPEC.md`. `SPEC.md` harus tetap jadi **dokumen inti/kesimpulan** yang ringkas — bukan tempat menyimpan detail penuh satu fitur.

Pola yang sudah berjalan di proyek ini:

| File | Isi |
|---|---|
| `SPEC.md` | Inti/kesimpulan utuh — ringkasan tiap area + tautan ke file detail. Jangan biarkan file ini membengkak lagi. |
| `AUTH.md` | Autentikasi, Google+Facebook OAuth, penautan akun |
| `ORGANISASI.md` | Organisasi, Struktur Organisasi, Tahun Angkatan, aset logo |
| `ARTIKEL.md` | Artikel, workflow editorial, struktur data, analitik |
| `KOMENTAR.md` | Komentar (termasuk Guest), filter kata kasar, moderasi |
| `LANDING-PAGE.md` | Landing page, akses edit, grid organisasi, dashboard |
| `SCROLL-PAGES.md` | Splash/Intro Screen (scroll parallax) — termasuk kode referensi as-received dari pengguna |

## Langkah Saat Menambah/Mengubah Fitur

1. **Fitur baru sama sekali** → buat file `.md` baru bernama sesuai fitur (`NAMA-FITUR.md`, huruf besar, kebab-case), isi lengkap dan mandiri (self-contained) — cukup detail untuk dipakai langsung sebagai acuan saat build/SDD tanpa perlu tanya ulang ke pengguna. Lalu tambahkan **ringkasan singkat + tautan** ke file itu di bagian relevan `SPEC.md` (§7 Ruang Lingkup Sistem, dan tabel "Dokumen Terkait" di bagian atas `SPEC.md`).
2. **Fitur yang sudah punya file** (mis. menambah detail ke Komentar) → edit file fitur itu langsung (`KOMENTAR.md`, dst.), bukan `SPEC.md`. Update ringkasan di `SPEC.md` hanya kalau ringkasannya jadi tidak akurat lagi.
3. **Catat setiap perubahan** di bagian "Riwayat Perubahan" `SPEC.md` — satu baris bernomor, tandai `[dikonfirmasi]` kalau sudah diputuskan bersama pengguna atau `[usulan, belum diputuskan]` kalau baru kandidat.
4. **Kandidat yang belum diputuskan pengguna** (misalnya diambil dari referensi proyek lama) masuk ke bagian "Usulan Tambahan" di `SPEC.md`, JANGAN ditulis seolah-olah sudah final di file fitur manapun sampai pengguna mengonfirmasi.
5. **Jangan pernah memperlakukan proyek lama** (`frontend-pemuda`/`backend-pemuda`, folder `specs/` bergaya spec-kit) **sebagai checklist wajib.** Boleh dipakai sebagai referensi keputusan yang sudah teruji, tapi setiap adopsinya ke proyek ini tetap butuh konfirmasi eksplisit dari pengguna.

## Kenapa Aturan Ini Ada

Riwayat proyek ini: `SPEC.md` sempat membengkak jadi satu file panjang berisi semua detail (autentikasi, organisasi, artikel, komentar, landing page, splash screen sekaligus), sampai pengguna secara eksplisit meminta dipecah supaya "SPEC.md tidak over" dan supaya AI yang membangun SDD nanti "paham dengan warna [nuansa] dan sesuai yang diinginkan". Pola file-per-fitur ini adalah solusinya — pertahankan pola ini untuk penambahan berikutnya, jangan kembali ke satu file raksasa.
