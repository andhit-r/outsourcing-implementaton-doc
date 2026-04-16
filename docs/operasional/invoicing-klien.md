---
title: Invoicing ke Klien
description: Panduan membuat invoice penagihan biaya tenaga kerja kepada klien berdasarkan data penggajian
---

# Invoicing ke Klien

Setelah penggajian karyawan selesai diproses, langkah berikutnya adalah **menagih klien** sesuai biaya tenaga kerja yang telah dikeluarkan vendor. Proses invoicing dibuat secara manual di modul Akuntansi Odoo.

---

## Konsep Billing Outsourcing

Dalam model bisnis outsourcing, vendor (PT. Maju Bersama) menagih klien berdasarkan:

```
Nilai Invoice ke Klien = Total Gaji Karyawan + Biaya Overhead + Margin Keuntungan
```

Struktur tagihan yang umum digunakan:

| Komponen Tagihan | Keterangan |
|---|---|
| **Biaya Gaji Kotor** | Total penghasilan karyawan (sebelum potongan BPJS, dll.) |
| **Biaya BPJS Perusahaan** | BPJS yang ditanggung vendor sebagai pemberi kerja |
| **Biaya Administrasi** | Biaya pengelolaan dan administrasi SDM |
| **Margin Keuntungan** | Keuntungan vendor dari jasa penempatan |

!!! note "Kebijakan Billing Berbeda-beda"
    Setiap vendor bisa memiliki kebijakan billing yang berbeda. Halaman ini hanya memberikan panduan umum — sesuaikan dengan kebijakan bisnis perusahaan Anda dan perjanjian dengan klien.

---

## Persiapan Sebelum Membuat Invoice

### 1. Rekap Karyawan per Klien

Sebelum membuat invoice, rekap terlebih dahulu:

**Langkah:** Buka `Pegawai > Penugasan Karyawan Eksternal`, filter dengan:
- **Status:** Aktif (Open)
- **Klien:** Pilih klien yang akan ditagih

Catat daftar karyawan yang aktif di klien tersebut selama periode yang ditagihkan.

!!! example "Rekap Karyawan Aktif — PT. Karya Utama — Januari 2025"

    | Karyawan | Posisi | Tanggal Mulai Penugasan | Status |
    |---|---|---|---|
    | Budi Santoso | Operator Produksi | 01/01/2025 | Aktif |
    | Sari Dewi | Operator Produksi | 15/11/2024 | Aktif |

---

### 2. Rekap Biaya Gaji per Klien

Dari slip gaji yang sudah selesai, rekap total biaya untuk karyawan di klien tersebut.

!!! example "Rekap Biaya Gaji — Karyawan di PT. Karya Utama — Januari 2025"

    | Karyawan | Gaji Pokok | Tunjangan | Total Gross | BPJS Perusahaan | Total Beban Vendor |
    |---|---|---|---|---|---|
    | Budi Santoso | Rp 4.000.000 | Rp 800.000 | Rp 4.800.000 | Rp 390.000 | Rp 5.190.000 |
    | Sari Dewi | Rp 3.800.000 | Rp 700.000 | Rp 4.500.000 | Rp 370.000 | Rp 4.870.000 |
    | **Total** | **Rp 7.800.000** | **Rp 1.500.000** | **Rp 9.300.000** | **Rp 760.000** | **Rp 10.060.000** |

---

### 3. Tentukan Nilai Invoice

Berdasarkan rekap biaya dan kebijakan billing, hitung nilai invoice:

!!! example "Perhitungan Nilai Invoice ke PT. Karya Utama"

    | Item | Perhitungan | Nilai |
    |---|---|---|
    | Total Beban Gaji | Seperti rekap di atas | Rp 10.060.000 |
    | Biaya Administrasi (5%) | 5% × Rp 10.060.000 | Rp 503.000 |
    | Margin Vendor (10%) | 10% × Rp 10.060.000 | Rp 1.006.000 |
    | **Total Invoice (sebelum PPN)** | | **Rp 11.569.000** |
    | PPN 11% | | Rp 1.272.590 |
    | **Total Invoice (termasuk PPN)** | | **Rp 12.841.590** |

---

## Membuat Invoice di Odoo

**Menu:** `Akuntansi > Pelanggan > Invoice > Baru`

### Mengisi Form Invoice

| Field | Nilai |
|---|---|
| **Pelanggan** | Pilih klien (mis. `PT. Karya Utama`) |
| **Tanggal Invoice** | Tanggal invoice diterbitkan |
| **Tanggal Jatuh Tempo** | Sesuai perjanjian (mis. 30 hari setelah tanggal invoice) |

**Di bagian Item Invoice**, tambahkan baris tagihan:

!!! example "Contoh Baris Invoice"

    | Produk/Deskripsi | Qty | Harga Satuan | Total |
    |---|---|---|---|
    | Jasa Tenaga Kerja Outsource - Operator Produksi - Januari 2025 | 2 | Rp 5.784.500 | Rp 11.569.000 |

    Atau bisa juga dirinci per karyawan:

    | Produk/Deskripsi | Qty | Harga Satuan | Total |
    |---|---|---|---|
    | Jasa TK Outsource - Budi Santoso - Jan 2025 | 1 | Rp 6.113.000 | Rp 6.113.000 |
    | Jasa TK Outsource - Sari Dewi - Jan 2025 | 1 | Rp 5.456.000 | Rp 5.456.000 |

---

### Konfirmasi Invoice

1. Review semua baris invoice
2. Klik **Konfirmasi** (Confirm)  
3. Nomor invoice digenerate otomatis
4. Status berubah ke **Diposting (Posted)**

---

### Kirim Invoice ke Klien

Setelah invoice dikonfirmasi:

1. Klik **Kirim & Cetak** (Send & Print)
2. Pilih cara pengiriman (email atau cetak PDF)
3. Invoice terkirim ke klien

---

## Memantau Status Pembayaran

**Menu:** `Akuntansi > Pelanggan > Invoice`

Filter berdasarkan:
- **Pelanggan:** nama klien
- **Status:** Dikirim (tidak terbayar)

### Mencatat Pembayaran

Ketika klien membayar:

1. Buka invoice yang sudah dibayar
2. Klik **Catat Pembayaran** (Register Payment)
3. Pilih rekening bank penerima
4. Masukkan tanggal dan referensi pembayaran
5. Konfirmasi

Invoice akan otomatis berubah ke status **Lunas (Paid)**.

---

## Laporan dan Rekonsiliasi

Untuk memastikan semua penagihan sudah terlaksana, lakukan rekonsiliasi bulanan:

| Pengecekan | Cara |
|---|---|
| Semua batch gaji sudah selesai | `Penggajian > Batch Slip Gaji` → filter bulan ini |
| Semua klien aktif sudah diinvoice | `Akuntansi > Invoice` → filter bulan ini → bandingkan dengan daftar klien aktif |
| Tidak ada invoice yang belum dibayar lebih dari jatuh tempo | `Akuntansi > Pelanggan > Invoice` → filter "Jatuh Tempo" |

---

!!! warning "Penting: Invoice Terpisah per Klien"
    Buat invoice yang **terpisah** untuk setiap klien. Jangan menggabungkan tagihan ke beberapa klien dalam satu invoice.

!!! tip "Lampiran Rekap Gaji"
    Banyak klien meminta **lampiran rekap gaji** sebagai bukti pendukung invoice. Lampirkan rekap Excel atau laporan dari Odoo berisi nama karyawan dan rincian biaya saat mengirim invoice.
