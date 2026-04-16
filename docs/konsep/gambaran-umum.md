---
title: Gambaran Umum Sistem
description: Penjelasan umum tentang sistem penggajian outsource menggunakan modul Odoo 14 SSI
---

# Gambaran Umum Sistem

## Konteks Bisnis

Perusahaan penyedia jasa outsourcing (disebut **vendor**) mengelola karyawan yang ditempatkan di berbagai perusahaan klien. Vendor bertanggung jawab atas:

- Penggajian karyawan setiap bulan
- Pencatatan penempatan (penugasan) karyawan di klien
- Penagihan biaya tenaga kerja kepada klien

Sistem Odoo yang dikonfigurasi dengan modul SSI memungkinkan seluruh proses ini berjalan terintegrasi dalam satu platform.

---

## Pihak-Pihak yang Terlibat

```mermaid
graph LR
    V["🏢 Vendor<br/>(PT. Maju Bersama)"]
    K1["🏭 Klien A<br/>(PT. Karya Utama)"]
    K2["🏬 Klien B<br/>(PT. Nusantara Jaya)"]
    E1["👷 Karyawan 1<br/>(Budi Santoso)"]
    E2["👷 Karyawan 2<br/>(Sari Dewi)"]
    E3["👷 Karyawan 3<br/>(Ahmad Fauzi)"]

    V -->|"Menugaskan"| E1
    V -->|"Menugaskan"| E2
    V -->|"Menugaskan"| E3
    E1 -->|"Ditempatkan di"| K1
    E2 -->|"Ditempatkan di"| K1
    E3 -->|"Ditempatkan di"| K2
    V -->|"Invoice biaya TK"| K1
    V -->|"Invoice biaya TK"| K2
```

| Pihak | Peran dalam Sistem |
|---|---|
| **Vendor** | Pengguna utama sistem Odoo. Mengelola karyawan, penugasan, penggajian, dan invoicing. |
| **Karyawan** | Tercatat sebagai karyawan vendor, ditempatkan di klien. Menerima gaji dari vendor. |
| **Klien** | Tercatat sebagai mitra (partner) di Odoo. Menerima invoice dari vendor. |

---

## Modul dan Fungsinya

### 1. Penugasan Karyawan Eksternal (`ssi_employee_external_assignment`)

Modul ini mencatat di mana seorang karyawan sedang ditempatkan. Setiap karyawan hanya boleh memiliki **satu penugasan aktif** pada satu waktu.

Informasi yang dicatat meliputi:
- Karyawan yang ditugaskan
- Klien tempat karyawan ditempatkan
- Tanggal mulai dan selesai penugasan
- Lokasi spesifik (jika klien memiliki beberapa lokasi)

### 2. Perjanjian Penggajian (`ssi_payroll_agreement`)

Modul ini mencatat kesepakatan gaji antara vendor dan karyawan. Setiap karyawan hanya boleh memiliki **satu perjanjian gaji aktif** pada satu waktu.

Informasi yang dicatat meliputi:
- Struktur gaji yang berlaku untuk karyawan ini
- Komponen gaji yang disepakati
- Input/nilai khusus (misal: tunjangan tetap tertentu)
- Dokumen perjanjian resmi

### 3. Slip Gaji (`ssi_hr_payroll`)

Modul ini memproses penggajian karyawan setiap periode. Sistem secara otomatis menghitung total gaji berdasarkan struktur gaji dan komponen yang telah dikonfigurasi.

Proses meliputi:
- Pembuatan slip gaji per karyawan
- Perhitungan otomatis komponen gaji
- Persetujuan dan pengesahan slip gaji
- Pencatatan akuntansi

### 4. Batch Gaji (`ssi_hr_payroll_batch`)

Ekstensi dari modul slip gaji yang memungkinkan pemrosesan gaji untuk banyak karyawan sekaligus dalam satu siklus persetujuan.

---

## Alur Data Antar Modul

```mermaid
flowchart TD
    A["Karyawan terdaftar\ndi Odoo"] --> B["Tipe Penugasan\ndikonfigurasi"]
    B --> C["Penugasan Karyawan\nke Klien dibuat"]
    C --> D["Perjanjian Gaji\ndibuat dan disetujui"]
    D --> E["Slip Gaji\ndibuat setiap bulan"]
    E --> F["Slip Gaji\ndisetujui & dikonfirmasi"]
    F --> G["Pencatatan\nAkuntansi Otomatis"]
    F --> H["Invoice ke Klien\ndibuat manual"]
    H --> I["Klien membayar\nInvoice"]

    style A fill:#e3f2fd
    style G fill:#e8f5e9
    style I fill:#e8f5e9
```

---

## Prinsip Kerja Sistem

### Status Dokumen

Semua dokumen dalam sistem mengikuti pola **status yang seragam**:

| Status | Keterangan |
|---|---|
| **Draft** | Dokumen baru dibuat, masih bisa diedit bebas |
| **Konfirmasi** | Dokumen dikirim untuk disetujui, tidak bisa diedit sembarangan |
| **Disetujui / Open** | Dokumen aktif dan berlaku |
| **Selesai / Done** | Dokumen selesai diproses |
| **Dibatalkan** | Dokumen dibatalkan |
| **Ditolak** | Dokumen dikembalikan untuk diperbaiki |

### Alur Persetujuan

Setiap dokumen penting memiliki alur persetujuan (approval flow). Ini memastikan:

- Tidak ada perubahan yang tidak sah
- Ada jejak audit yang jelas
- Setiap tindakan dapat dilacak siapa yang melakukannya

### Penomoran Dokumen Otomatis

Nomor dokumen (seperti nomor slip gaji, nomor perjanjian) digenerate otomatis oleh sistem ketika dokumen mencapai status tertentu. Implementor perlu mengkonfigurasi **sequence** (urutan penomoran) untuk setiap jenis dokumen.

---

!!! note "Catatan untuk Implementor"
    Sebelum memulai konfigurasi, pastikan modul-modul berikut sudah terinstall di Odoo:
    
    - `ssi_employee_external_assignment`
    - `ssi_payroll_agreement`
    - `ssi_hr_payroll`
    - `ssi_hr_payroll_batch` (opsional, untuk proses massal)
    
    Instalasi modul adalah tanggung jawab tim teknis, bukan implementor.
