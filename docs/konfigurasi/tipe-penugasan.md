---
title: Konfigurasi Tipe Penugasan Karyawan
description: Cara mengkonfigurasi tipe penugasan karyawan eksternal untuk berbagai jenis klien
---

# Konfigurasi Tipe Penugasan Karyawan

**Tipe Penugasan** adalah konfigurasi yang menentukan **karyawan mana** dan **klien mana** yang bisa dipilih saat membuat dokumen penugasan. Konfigurasi ini sangat penting untuk menjaga agar penugasan yang dibuat selalu sesuai aturan bisnis.

---

## Konsep Tipe Penugasan

Setiap dokumen Penugasan Karyawan Eksternal harus memilih satu **Tipe Penugasan**. Tipe ini mengatur:

1. **Filter Karyawan** — Siapa saja karyawan yang bisa dipilih untuk tipe penugasan ini
2. **Filter Klien** — Klien mana saja yang bisa dijadikan tujuan penugasan

!!! example "Mengapa Perlu Filter?"
    Misalnya, perusahaan outsourcing Anda memiliki dua jenis klien:
    
    - **Klien Industri** → hanya menerima karyawan dengan posisi Operator atau Teknisi
    - **Klien Perkantoran** → hanya menerima karyawan dengan posisi Staf Admin atau Resepsionis
    
    Dengan mengkonfigurasi tipe penugasan yang berbeda, sistem akan **mencegah kesalahan** saat HRD membuat dokumen penugasan.

---

## Metode Filter yang Tersedia

Untuk filter karyawan maupun klien, tersedia 3 metode:

| Metode | Cara Kerja | Cocok untuk |
|---|---|---|
| **Daftar Manual** | Pilih karyawan/klien satu per satu | Daftar tetap dan jarang berubah |
| **Domain Filter** | Filter otomatis berdasarkan kriteria (jabatan, departemen, dll.) | Daftar yang terus bertambah dengan pola yang sama |
| **Kode Evaluasi** | Logika seleksi yang lebih kompleks | Aturan bisnis yang tidak bisa diekspresikan dengan filter biasa |

---

## Langkah Membuat Tipe Penugasan

**Menu:** `Pegawai > Konfigurasi > Tipe Penugasan Eksternal`

### Contoh 1: Penugasan Operator ke Klien Industri

Skenario: Hanya karyawan dengan jabatan "Operator" yang bisa dikirim ke klien yang berjenis "Industri Manufaktur".

| Field | Nilai |
|---|---|
| **Nama** | `Penugasan Operator - Klien Industri` |
| **Metode Filter Karyawan** | Domain |
| **Domain Karyawan** | `[('job_id.name', 'ilike', 'Operator')]` |
| **Metode Filter Klien** | Domain |
| **Domain Klien** | `[('industry_id.name', '=', 'Industri Manufaktur')]` |

!!! note "Catatan Domain"
    Domain di Odoo mengikuti format standar Odoo domain. Minta bantuan tim teknis jika perlu menetapkan domain yang kompleks.

---

### Contoh 2: Penugasan Staf Admin ke Semua Klien Perkantoran

| Field | Nilai |
|---|---|
| **Nama** | `Penugasan Staf Admin - Klien Perkantoran` |
| **Metode Filter Karyawan** | Domain |
| **Domain Karyawan** | `[('job_id.name', 'ilike', 'Staf')]` |
| **Metode Filter Klien** | Daftar Manual |
| **Klien yang Diizinkan** | PT. Nusantara Jaya, CV. Berkah Abadi |

---

### Contoh 3: Penugasan Umum (Tanpa Filter Ketat)

Untuk perusahaan kecil yang belum membutuhkan filter ketat:

| Field | Nilai |
|---|---|
| **Nama** | `Penugasan Umum` |
| **Metode Filter Karyawan** | Daftar Manual |
| **Karyawan yang Diizinkan** | (pilih semua karyawan aktif) |
| **Metode Filter Klien** | Daftar Manual |
| **Klien yang Diizinkan** | (pilih semua klien) |

---

## Rekomendasi Nama Tipe Penugasan

Gunakan format penamaan yang konsisten agar mudah dikelola:

```
[Posisi/Jenis Karyawan] - [Jenis Klien]
```

Contoh:
- `Operator Produksi - Industri Manufaktur`
- `Staf Admin - Perkantoran Umum`
- `Satpam - Semua Klien`
- `Cleaning Service - Perkantoran dan Industri`

---

## Checklist Tipe Penugasan

Setelah konfigurasi, verifikasi hal-hal berikut:

- [ ] Setiap jenis kombinasi karyawan & klien sudah memiliki tipe penugasan yang sesuai
- [ ] Uji coba membuat dokumen penugasan dengan tipe yang baru dibuat
- [ ] Pastikan filter karyawan menampilkan karyawan yang tepat
- [ ] Pastikan filter klien menampilkan klien yang tepat
- [ ] Pastikan karyawan di luar filter tidak bisa dipilih

!!! warning "Tipe Penugasan Tidak Bisa Mudah Diganti"
    Setelah dokumen penugasan dibuat dan disetujui, tipe penugasannya tidak bisa diubah. Pastikan tipe penugasan sudah benar sejak awal.
