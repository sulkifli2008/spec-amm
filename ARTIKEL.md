# Artikel & Alur Editorial — Detail

Dokumen pelengkap `SPEC.md` (§8–§13) — detail lengkap CRUD artikel, alur verifikasi, struktur data, dan analitik, dipisah ke file ini supaya SPEC.md tetap ringkas sebagai ringkasan/kesimpulan utuh.

## 1. Functional Requirements

- **Artikel**: Buat, Edit, Hapus, Simpan Draft, Submit Review, Publish, Unpublish, Upload Thumbnail/Gambar/Lampiran, Preview.
- **Verifikasi**: Approve, Reject, Catatan Revisi (Super Admin).

## 2. Workflow Artikel

```
Draft → Submit Review → Pending Review → Review Super Admin
                                              │
                              ┌───────────────┴───────────────┐
                              ▼                               ▼
                          Approve                          Reject
                              │                               │
                          Published                   Revisi → Submit Review
```

## 3. Status Artikel

Draft, Pending Review, Approved, Rejected, Published, Unpublished, Archived

## 4. Struktur Artikel (data)

Judul, Slug, Thumbnail, Isi Artikel, Organisasi Penerbit, Penulis, Kategori, Tag, Status, Waktu Publish, Waktu Update, Jumlah Like/Unlike/Komentar/Pembaca, SEO Title, Meta Description

## 5. Detail Artikel (tampilan)

Judul, Thumbnail, Isi, Organisasi, Penulis, Tanggal Publish, Waktu Baca, Tag, Share Media Sosial, Like, Unlike, Komentar + Balasan (termasuk komentar Guest, lihat `KOMENTAR.md`), Artikel Terkait

## 6. Analitik Dashboard Organisasi

Total Artikel, Total Pembaca, Total Like, Total Unlike, Total Komentar, Artikel Terpopuler, Artikel Terbaru, Artikel Pending, Artikel Ditolak — ditampilkan di dashboard Organisasi, dibatasi ke organisasi yang sedang login.
