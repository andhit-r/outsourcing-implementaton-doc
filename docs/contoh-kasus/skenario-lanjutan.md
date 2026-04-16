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

| Karyawan | Posisi | Ditempatkan di | Skema Gaji |
|---|---|---|---|
| Budi Santoso | Operator Produksi | PT. Karya Utama | Gaji Operator |
| Sari Dewi | Operator Produksi | PT. Karya Utama | Gaji Operator |
| Ahmad Fauzi | Operator Produksi | PT. Karya Utama | Gaji Operator |
| Rina Puspita | Staf Administrasi | PT. Karya Utama | Gaji Staf Admin |
| Doni Kurniawan | Staf Keuangan | PT. Nusantara Jaya | Gaji Staf Senior |
| Maya Sari | Staf Administrasi | PT. Nusantara Jaya | Gaji Staf Admin |
| Hendra Wijaya | Staf IT | PT. Nusantara Jaya | Gaji Staf Senior |
| Tono Santoso | Satpam | CV. Berkah Abadi | Gaji Satpam |
| Wati Rahayu | Satpam | CV. Berkah Abadi | Gaji Satpam |
| Agus Priyono | Cleaning Service | CV. Berkah Abadi | Gaji CS |
| Dewi Lestari | Cleaning Service | CV. Berkah Abadi | Gaji CS |
| Fajar Nugroho | Supervisor | PT. Karya Utama | Gaji Supervisor |

---

## Tantangan Implementasi

Skenario ini lebih kompleks karena:

1. **4 skema gaji berbeda** — Operator, Staf Admin, Staf Senior, Satpam, CS, Supervisor
2. **3 klien berbeda** — dengan struktur tagihan dan margin yang berbeda
3. **Karyawan yang pindah klien** — Rina Puspita sebelumnya di PT. Nusantara Jaya, pindah ke PT. Karya Utama mulai Februari
4. **Kenaikan gaji** — Fajar Nugroho naik jabatan dari Operator ke Supervisor mulai Februari

---

## Konfigurasi yang Diperlukan

### Struktur Gaji yang Dibutuhkan

```
Struktur Gaji Dasar Outsource (induk)
├── Gaji Operator Produksi
├── Gaji Staf Administrasi
├── Gaji Staf Senior
├── Gaji Satpam
├── Gaji Cleaning Service
└── Gaji Supervisor
```

### Tipe Penugasan yang Diperlukan

| Tipe Penugasan | Digunakan Untuk |
|---|---|
| `Operator & Teknisi - Klien Industri` | Karyawan posisi Operator/Teknisi ke klien manufaktur |
| `Staf Kantor - Klien Keuangan & Industri` | Staf Admin/Keuangan/IT ke klien umum |
| `Satpam - Semua Klien` | Satpam ke semua klien |
| `Cleaning Service - Semua Klien` | CS ke semua klien |
| `Supervisor - Klien Premium` | Supervisor ke klien tertentu saja |

---

## Menangani Perubahan di Februari 2025

### Kasus 1: Rina Puspita Pindah Klien

Rina sebelumnya ditempatkan di PT. Nusantara Jaya, dan mulai Februari dipindahkan ke PT. Karya Utama.

**Langkah yang Dilakukan:**

1. **Akhiri penugasan lama Rina di PT. Nusantara Jaya**
   - Buka penugasan aktif Rina
   - Klik **Hentikan**
   - Alasan: `Pemindahan penugasan ke klien lain`

2. **Buat penugasan baru Rina di PT. Karya Utama**
   - Tipe Penugasan: `Staf Kantor - Klien Keuangan & Industri`
   - Karyawan: `Rina Puspita`
   - Klien: `PT. Karya Utama`
   - Tanggal Mulai: `01/02/2025`

3. **Perjanjian gaji Rina** tidak perlu diubah (skema gajinya sama, hanya kliennya berbeda).

!!! warning "Penting: Penugasan dan Perjanjian adalah Dokumen Terpisah"
    Perubahan penugasan (ke klien mana ditempatkan) TIDAK otomatis mengubah perjanjian gaji. Perubahan perjanjian gaji hanya diperlukan jika skema gaji karyawan berubah.

---

### Kasus 2: Fajar Nugroho Naik Jabatan

Fajar naik dari Operator ke Supervisor, dengan perubahan skema gaji dan nilai gaji.

**Langkah yang Dilakukan:**

1. **Akhiri perjanjian gaji lama Fajar (skema Operator)**
   - Pastikan slip gaji Januari menggunakan perjanjian lama
   - Perjanjian lama akan otomatis selesai saat perjanjian baru diaktifkan

2. **Buat perjanjian gaji baru Fajar (skema Supervisor)**
   - Tipe Perjanjian: `Perjanjian Outsource Supervisor`
   - Karyawan: `Fajar Nugroho`
   - Tanggal: `01/02/2025`
   - Struktur Gaji: `Gaji Supervisor`
   - Input: Gaji Pokok baru (misalnya Rp 7.000.000), Tunjangan Jabatan, dll.

3. **Aktifkan perjanjian baru** → Perjanjian lama otomatis selesai

4. Penugasan Fajar di PT. Karya Utama **tidak perlu diubah** (masih di klien yang sama, hanya jabatan yang berubah).

---

## Pemrosesan Gaji Februari 2025

### Strategi: Batch per Klien

Karena ada 3 klien, lebih efisien membuat **3 batch terpisah** agar mudah saat invoicing.

#### Batch 1: PT. Karya Utama

**Karyawan yang dimasukkan:**
- Budi Santoso (Operator)
- Sari Dewi (Operator)
- Ahmad Fauzi (Operator)
- Rina Puspita (Staf Admin) ← pindahan dari Nusantara Jaya
- Fajar Nugroho (Supervisor) ← skema gaji baru

**Nama Batch:** `Gaji Februari 2025 - PT. Karya Utama`

!!! example "Verifikasi Khusus untuk Bulan Ini"
    Karena ada perubahan, pastikan:
    
    - Rina Puspita hanya muncul di batch PT. Karya Utama (tidak di batch PT. Nusantara Jaya)
    - Fajar Nugroho menggunakan struktur gaji `Gaji Supervisor` (bukan `Gaji Operator`)

#### Batch 2: PT. Nusantara Jaya

**Karyawan:** Doni Kurniawan, Maya Sari, Hendra Wijaya (3 orang)  
**Nama Batch:** `Gaji Februari 2025 - PT. Nusantara Jaya`

#### Batch 3: CV. Berkah Abadi

**Karyawan:** Tono Santoso, Wati Rahayu, Agus Priyono, Dewi Lestari (4 orang)  
**Nama Batch:** `Gaji Februari 2025 - CV. Berkah Abadi`

---

## Invoicing Multi-Klien

Karena batch sudah dibuat per klien, proses invoicing menjadi lebih mudah.

### Invoice 1 — PT. Karya Utama

Ambil data dari **Batch Gaji Februari 2025 - PT. Karya Utama** (5 karyawan):

!!! example "Rekap Biaya PT. Karya Utama — Februari 2025"

    | Karyawan | Posisi | Total Beban Vendor |
    |---|---|---|
    | Budi Santoso | Operator | Rp 5.188.000 |
    | Sari Dewi | Operator | Rp 4.970.000 |
    | Ahmad Fauzi | Operator | Rp 5.407.000 |
    | Rina Puspita | Staf Admin | Rp 6.200.000 |
    | Fajar Nugroho | Supervisor | Rp 9.100.000 |
    | **Total Beban** | | **Rp 30.865.000** |

    **Nilai Invoice (+ biaya admin 5% + margin 10% + PPN 11%):**
    
    Total Invoice = Rp 30.865.000 × 1.15 × 1.11 = **Rp 39.419.000** *(dibulatkan)*

---

### Invoice 2 — PT. Nusantara Jaya

!!! example "Rekap Biaya PT. Nusantara Jaya — Februari 2025"

    | Karyawan | Posisi | Total Beban Vendor |
    |---|---|---|
    | Doni Kurniawan | Staf Keuangan | Rp 7.500.000 |
    | Maya Sari | Staf Admin | Rp 5.800.000 |
    | Hendra Wijaya | Staf IT | Rp 8.200.000 |
    | **Total Beban** | | **Rp 21.500.000** |

    **Nilai Invoice:** Rp 21.500.000 × 1.15 × 1.11 = **Rp 27.456.000** *(dibulatkan)*

---

### Invoice 3 — CV. Berkah Abadi

!!! example "Rekap Biaya CV. Berkah Abadi — Februari 2025"

    | Karyawan | Posisi | Total Beban Vendor |
    |---|---|---|
    | Tono Santoso | Satpam | Rp 4.200.000 |
    | Wati Rahayu | Satpam | Rp 4.200.000 |
    | Agus Priyono | Cleaning Service | Rp 3.500.000 |
    | Dewi Lestari | Cleaning Service | Rp 3.500.000 |
    | **Total Beban** | | **Rp 15.400.000** |

    **Nilai Invoice:** Rp 15.400.000 × 1.12 × 1.11 = **Rp 19.169.000** *(dibulatkan, margin 12% untuk klien ini)*

---

## Ringkasan Skenario Lanjutan

Dari skenario ini kita bisa melihat bahwa sistem menangani dengan baik:

- ✅ Banyak karyawan dengan skema gaji berbeda
- ✅ Karyawan pindah klien di tengah bulan
- ✅ Karyawan naik jabatan dengan perubahan skema gaji
- ✅ Batch terpisah per klien untuk memudahkan invoicing
- ✅ Margin berbeda per klien

---

!!! tip "Lessons Learned dari Skenario Ini"
    
    1. **Selalu cek perubahan sebelum buat batch** – buat checklist: ada karyawan yang pindah klien? ada yang naik jabatan? ada yang keluar?
    
    2. **Batch per klien sangat membantu** – waktu invoicing jadi lebih cepat karena data sudah terkelompok
    
    3. **Perjanjian gaji adalah kunci akurasi** – pastikan setiap karyawan selalu ada perjanjian aktif sebelum proses gaji dimulai
    
    4. **Rekonsiliasi di akhir bulan** – selalu cocokkan jumlah karyawan aktif (dari data penugasan) dengan jumlah slip gaji yang diproses
