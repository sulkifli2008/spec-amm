# Komentar & Moderasi — Detail

Dokumen pelengkap `SPEC.md` (§8) — detail lengkap komentar Guest, filter kata kasar, dan moderasi, dipisah ke file ini supaya SPEC.md tetap ringkas sebagai ringkasan/kesimpulan utuh. Status semua bagian di bawah: **[v1.1 — dikonfirmasi]**, kecuali ditandai lain.

## 1. Komentar Guest (nama otomatis per-perangkat)

**[v1.1 — dikonfirmasi, koreksi dari PRD awal]** Guest (belum login) boleh menambah komentar — sebelumnya di PRD v1.0 komentar/like hanya untuk User yang login.

- Komentar Guest tersimpan dengan nama otomatis per-perangkat (mis. "Tamu-1234").
- Nama itu **konsisten** untuk perangkat/browser yang sama pada kunjungan berikutnya (dibaca dari penanda lokal per-device, bukan di-generate ulang tiap kali).
- Begitu Guest tersebut login (jadi User), komentar-komentar sebelumnya **otomatis berganti** memakai nama akun, bukan lagi nama tamu.

## 2. Filter Kata Kasar

Berlaku untuk **semua** pengirim komentar (Guest, User, Organisasi yang membalas), bukan cuma Guest:

- Mendeteksi kata kasar dasar **dan** variasi penulisan/leetspeak (mis. "anjing" maupun "4nj1ng").
- Kata sah yang kebetulan mengandung substring terlarang (mis. "asuransi", "masuk") **tidak boleh** salah ditolak — pencocokan harus kata utuh, bukan substring bebas.

## 3. Fungsi Komentar (Functional Requirements)

- **Komentar**: Tambah, Balas (nested reply satu level), Hapus, Like, Unlike, Report.
- Laporan komentar (**Report**) WAJIB menyertakan **alasan dari daftar tetap**, bukan teks bebas — supaya alasan laporan sendiri tidak perlu dimoderasi lagi.

## 4. Moderasi Komentar (Super Admin)

Super Admin meninjau antrian komentar yang dilaporkan (beserta alasan & pelapor) dan menindaklanjuti dengan dua kemungkinan tindakan:

- **Abaikan** — membersihkan status laporan, komentar tetap tayang.
- **Hapus** — komentar dihapus (soft delete).

## 5. Non-Functional terkait

Rate Limiting untuk komentar (mencegah spam), Filter kata kasar pada komentar — lihat `SPEC.md` §14 untuk daftar NFR lengkap lintas fitur.
