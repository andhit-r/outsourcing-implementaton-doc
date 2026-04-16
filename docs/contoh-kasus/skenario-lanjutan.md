---
title: Skenario Lanjutan — Multi-Klien, Multi-Skema Gaji
description: Contoh implementasi kompleks untuk perusahaan outsourcing dengan banyak klien dan berbagai skema gaji
---

# Skenario Lanjutan: Multi-Klien, Multi-Skema Gaji

Skenario ini menggambarkan implementasi yang lebih kompleks untuk **perusahaan outsourcing menengah** yang menempatkan karyawan di berbagai klien dengan jenis pekerjaan dan skema gaji yang berbeda-beda.

---

## Profil Skenario

| | Detail |
|---|---|
| **Vendor** | PT. Maju Bersama |
| **Klien 1** | PT. Karya Utama (industri manufaktur) |
| **Klien 2** | PT. Nusantara Jaya (perusahaan jasa keuangan) |
| **Klien 3** | CV. Berkah Abadi (properti & gedung perkantoran) |
| **Total Karyawan** | 12 orang, berbagai posisi |
| **Periode** | Februari 2025 |

---

## Distribusi Karyawan

| Karyawan | Posisi | Ditempatkan di | Perjanjian Outsource |
|---|---|---|---|
| Budi Santoso | Operator Produksi | PT. Karya Utama | EEAA/2025/000001 |
| Sari Dewi | Operator Produksi | PT. Karya Utama | EEAA/2025/000001 |
| Ahmad Fauzi | Operator Produksi | PT. Karya Utama | EEAA/2025/000001 |
| Rina Puspita | Staf Administrasi | PT. Karya Utama | EEAA/2025/000001 |
| Doni Kurniawan | Staf Keuangan | PT. Nusantara Jaya | EEAA/2025/000002 |
| Maya Sari | Staf Administrasi | PT. Nusantara Jaya | EEAA/2025/000002 |
| Hendra Wijaya | Staf IT | PT. Nusantara Jaya | EEAA/2025/000002 |
| Tono Santoso | Satpam | CV. Berkah Abadi | EEAA/2025/000003 |
| Wati Rahayu | Satpam | CV. Berkah Abadi | EEAA/2025/000003 |
| Agus Priyono | Cleaning Service | CV. Berkah Abadi | EEAA/2025/000003 |
| Dewi Lestari | Cleaning Service | CV. Berkah Abadi | EEAA/2025/000003 |
| Fajar Nugroho | Supervisor | PT. Karya Utama | EEAA/2025/000001 |

---

## Perjanjian Outsource per Klien

Setiap klien memiliki **satu perjanjian outsource** yang mendefinisikan posisi dan kompensasi:

### `EEAA/2025/000001` — PT. Karya Utama

| Posisi | Kuota | Komponen | Rentang |
|---|---|---|---|
| Operator Produksi | 5 | Gaji Pokok | Rp 3.800.000 – Rp 5.000.000 |
| Staf Administrasi | 2 | Gaji Pokok | Rp 4.500.000 – Rp 6.000.000 |
| Supervisor | 1 | Gaji Pokok | Rp 6.000.000 – Rp 9.000.000 |

### `EEAA/2025/000002` — PT. Nusantara Jaya

| Posisi | Kuota | Komponen | Rentang |
|---|---|---|---|
| Staf Keuangan | 2 | Gaji Pokok | Rp 5.500.000 – Rp 8.000.000 |
| Staf Administrasi | 2 | Gaji Pokok | Rp 4.500.000 – Rp 6.000.000 |
| Staf IT | 1 | Gaji Pokok | Rp 6.000.000 – Rp 9.000.000 |

### `EEAA/2025/000003` — CV. Berkah Abadi

| Posisi | Kuota | Komponen | Rentang |
|---|---|---|---|
| Satpam | 3 | Gaji Pokok | Rp 3.500.000 – Rp 4.500.000 |
| Cleaning Service | 3 | Gaji Pokok | Rp 3.000.000 – Rp 3.800.000 |

---

## Menangani Perubahan di Februari 2025

### Kasus 1: Rina Puspita Pindah Klien

Rina sebelumnya di PT. Nusantara Jaya, dipindahkan ke PT. Karya Utama mulai Februari.

**Langkah:**

1. **Akhiri penugasan lama** di PT. Nusantara Jaya (Terminate — alasan: Pemindahan)
2. **Buat penugasan baru** ke PT. Karya Utama:
   - Perjanjian Outsource: `EEAA/2025/000001`
   - Tanggal Mulai: `01/02/2025`
3. **Perjanjian gaji Rina tidak perlu diubah** (skema sama, hanya klien berbeda)

!!! warning "Penting"
    Pastikan penugasan lama **tidak lagi aktif** di bulan Februari, agar saat Termin Pembayaran PT. Nusantara Jaya di-load, Rina tidak ikut terhitung.

---

### Kasus 2: Fajar Nugroho Naik Jabatan

Fajar naik dari Operator ke Supervisor mulai Februari, dengan gaji baru.

**Langkah:**

1. **Buat perjanjian gaji baru** untuk Fajar (Supervisor):
   - Struktur Gaji: `Gaji Supervisor`
   - Input: Gaji Pokok = Rp 7.000.000, Tunjangan Jabatan = Rp 1.000.000
2. **Aktifkan perjanjian baru** → perjanjian lama otomatis Selesai
3. **Penugasan Fajar ke PT. Karya Utama tidak berubah** (masih klien yang sama)

---

## Pemrosesan Gaji Februari 2025

### Strategi: Satu Batch per Klien

Buat 3 batch terpisah — ini memudahkan loading data saat invoicing:

| Batch | Karyawan | Catatan |
|---|---|---|
| `Gaji Feb 2025 - PT. Karya Utama` | Budi, Sari, Ahmad, Rina, Fajar | 5 karyawan; Rina baru masuk bulan ini; Fajar pakai skema baru |
| `Gaji Feb 2025 - PT. Nusantara Jaya` | Doni, Maya, Hendra | 3 karyawan (Rina sudah tidak di sini) |
| `Gaji Feb 2025 - CV. Berkah Abadi` | Tono, Wati, Agus, Dewi | 4 karyawan |

!!! example "Checklist Sebelum Membuat Batch"
    - Rina hanya muncul di batch PT. Karya Utama ✓
    - Fajar menggunakan struktur `Gaji Supervisor` ✓
    - Tidak ada karyawan yang masuk dua batch sekaligus ✓

---

## Invoicing Februari 2025 — 3 Termin Pembayaran

Karena ada 3 perjanjian outsource, buat 3 termin pembayaran:

### Termin 1 — PT. Karya Utama (`EEAA/2025/000001`)

1. Buat Termin Pembayaran, Perjanjian: `EEAA/2025/000001`, Periode: `01/02 – 28/02/2025`
2. Load Penugasan → 5 karyawan ditemukan (termasuk Rina yang baru, tidak termasuk Rina dari Nusantara Jaya)
3. Load Slip Gaji → 5 slip gaji
4. Load Baris Slip Gaji → aturan pembayaran teragregasi

!!! example "Contoh Aturan Pembayaran (Preview)"
    | Komponen | Total 5 Karyawan |
    |---|---|
    | Gaji Pokok | Rp 23.800.000 |
    | Tunjangan Jabatan | Rp 1.000.000 |
    | Tunjangan Lainnya | Rp 4.000.000 |
    | BPJS Perusahaan | Rp 1.140.000 |
    | **Subtotal** | **Rp 29.940.000** |
    | **PPN 11%** | **Rp 3.293.400** |
    | **Total Invoice** | **Rp 33.233.400** |

5. Konfirmasi → Buat Invoice → Invoice `INV/2025/02/0001` ke PT. Karya Utama

---

### Termin 2 — PT. Nusantara Jaya (`EEAA/2025/000002`)

1. Buat Termin, Periode: `01/02 – 28/02/2025`
2. Load Penugasan → 3 karyawan (Rina tidak muncul karena penugasannya sudah dihentikan)
3. Load Slip Gaji → 3 slip gaji Doni, Maya, Hendra
4. Konfirmasi → Buat Invoice → Invoice ke PT. Nusantara Jaya

---

### Termin 3 — CV. Berkah Abadi (`EEAA/2025/000003`)

1. Buat Termin, Periode: `01/02 – 28/02/2025`
2. Load Penugasan → 4 karyawan
3. Load Slip Gaji → 4 slip gaji
4. Konfirmasi → Buat Invoice → Invoice ke CV. Berkah Abadi

---

## Ringkasan Skenario Lanjutan

Dari skenario ini kita melihat kemampuan sistem menangani:

- ✅ Banyak karyawan dengan skema gaji berbeda
- ✅ 3 perjanjian outsource terpisah per klien
- ✅ Karyawan pindah klien — termin pembayaran terkait otomatis konsisten
- ✅ Karyawan naik jabatan — perjanjian gaji baru otomatis menggantikan yang lama
- ✅ Invoice per klien terbuat otomatis — tidak ada input manual
- ✅ Margin berbeda per klien (dikonfigurasi di biaya lainnya per perjanjian)

---

!!! tip "Lessons Learned"
    1. **Setup perjanjian outsource di awal kontrak** — jangan tunggu mau invoicing baru dibuat
    2. **Hubungkan penugasan ke perjanjian outsource** saat membuat penugasan, bukan setelahnya
    3. **Batch per klien** membuat invoicing jauh lebih cepat dan akurat
    4. **Cek perubahan bulan ini sebelum batch** — ada yang pindah? naik jabatan? keluar?
    5. **Rekonsiliasi karyawan aktif vs slip gaji yang diproses** sebelum membuat termin pembayaran
