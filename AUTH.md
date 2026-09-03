# Autentikasi & Penautan Akun — Detail

Dokumen pelengkap `SPEC.md` (§5 Target Pengguna, §8 Functional Requirements) — detail lengkap jalur login/registrasi dan kebijakan penautan akun, dipisah ke file ini supaya SPEC.md tetap ringkas sebagai ringkasan/kesimpulan utuh. Status semua bagian di bawah: **[v1.1 — dikonfirmasi]**, kecuali ditandai lain.

## 1. Jalur Login per Role

- **Super Admin & Organisasi**: login email + password. Akun dibuat manual oleh Super Admin (lihat `ORGANISASI.md`) — tidak ada self-registrasi untuk kedua role ini.
- **User**: login via **Google OAuth** atau **Facebook OAuth** (PRD awal hanya menyebut Google — Facebook ditambahkan di v1.1).
- **Guest**: tidak butuh akun untuk membaca; sejak v1.1 juga tidak butuh akun untuk berkomentar (lihat `KOMENTAR.md` — nama otomatis per-perangkat).

## 2. Reset Password & Ganti Password

- **Reset Password**: alur lupa password untuk Super Admin/Organisasi (email+password). Detail teknis (token, kedaluwarsa, dsb) mengikuti standar keamanan §18.8 `SPEC.md` — belum dirinci lebih jauh di sesi ini, ikuti praktik umum (token sekali pakai, kedaluwarsa singkat, tidak pernah mengembalikan token di response).
- **Ganti Password**: form ganti password untuk pengguna yang sudah login.

## 3. Kebijakan Penautan Akun (Account Linking)

Identitas dari Google, Facebook, dan email/password disatukan berdasarkan **email terverifikasi**:

- Begitu email yang dikembalikan provider (Google/Facebook) **cocok dan berstatus terverifikasi** dengan akun yang sudah ada (dari jalur mana pun — provider lain atau email/password), sistem **otomatis login/mengaitkan ke akun yang sama** — tidak ada akun duplikat, dan tidak perlu layar konfirmasi tambahan, karena kedua provider sudah memverifikasi kepemilikan email sebelum membagikannya ke sistem ini.
- Kalau email dari provider **TIDAK berstatus terverifikasi** (kasus langka), sistem WAJIB menolak auto-link dan meminta pengguna login lewat email/password dulu, baru menghubungkan provider baru dari halaman Profil.
- **Tidak ada "perjanjian" khusus yang dibutuhkan antara Google dan Facebook** — keduanya provider OAuth yang independen satu sama lain; kebijakan penautan murni keputusan internal aplikasi ini, bukan sesuatu yang perlu dinegosiasikan dengan kedua platform.

## 4. Functional Requirements

- Login, Logout, Login Google & Facebook (OAuth), Reset Password, Ganti Password.
- **"Request Akun Organisasi" dihapus** dari Authentication — akun Organisasi dibuat langsung oleh Super Admin di Dashboard Super Admin > Organisasi (lihat `ORGANISASI.md`), tidak lagi lewat pengajuan Guest yang di-approve.
- **Penautan Akun (Account Linking)**: lihat §3 di atas.

## 5. Non-Functional terkait

JWT Authentication, OAuth Google & Facebook, Rate Limiting (login), Secure Cookie, Refresh Token — lihat `SPEC.md` §14 dan §18.8 untuk daftar keamanan lengkap lintas fitur.
