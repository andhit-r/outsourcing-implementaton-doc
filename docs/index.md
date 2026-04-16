---
title: Beranda
description: Dokumentasi implementasi pengelolaan penggajian karyawan outsource menggunakan modul Odoo 14 SSI
---

# Dokumentasi Implementasi Penggajian Outsource

Selamat datang di dokumentasi implementasi pengelolaan penggajian karyawan outsource menggunakan modul Odoo 14 yang dikembangkan oleh **PT. Simetri Sinergi Indonesia (OpenSynergy Indonesia)**.

---

## Tentang Dokumentasi Ini

Dokumentasi ini dirancang khusus untuk **implementor** — yaitu orang yang bertugas mengkonfigurasi dan mendeploykan sistem Odoo untuk perusahaan penyedia jasa outsourcing (vendor tenaga kerja).

Anda akan menemukan panduan langkah demi langkah untuk:

- Mengkonfigurasi sistem dari awal
- Mengelola penugasan karyawan ke klien
- Membuat perjanjian penggajian per karyawan
- Memproses slip gaji bulanan secara individual maupun massal
- Menagih klien berdasarkan data penggajian

---

## Skenario Bisnis yang Dicakup

=== "Perusahaan Outsourcing Kecil"

    Perusahaan dengan 10–50 karyawan outsource yang ditempatkan di 2–5 klien berbeda. Penggajian diproses satu per satu setiap bulan.

=== "Perusahaan Outsourcing Menengah"

    Perusahaan dengan 50–200 karyawan outsource yang ditempatkan di berbagai lokasi klien. Penggajian diproses secara batch (massal) setiap bulan.

=== "Multi-Klien dengan Skema Gaji Berbeda"

    Perusahaan yang menempatkan karyawan dengan skema gaji berbeda-beda tergantung klien dan jenis pekerjaan. Setiap karyawan memiliki perjanjian gaji sendiri.

---

## Modul yang Digunakan

| Modul | Fungsi |
|---|---|
| `ssi_employee_external_assignment` | Mencatat penugasan karyawan ke klien eksternal |
| `ssi_payroll_agreement` | Membuat perjanjian penggajian dengan karyawan |
| `ssi_hr_payroll` | Memproses slip gaji individual |
| `ssi_hr_payroll_batch` | Memproses slip gaji massal (batch) |

---

## Struktur Dokumentasi

<div class="grid cards" markdown>

-   :material-lightbulb-outline: **Konsep**

    ---

    Pelajari konsep dasar sistem terlebih dahulu sebelum mulai konfigurasi.

    [:octicons-arrow-right-24: Gambaran Umum](konsep/gambaran-umum.md)

-   :material-cog-outline: **Konfigurasi**

    ---

    Siapkan semua data master dan pengaturan sistem sebelum operasional.

    [:octicons-arrow-right-24: Mulai Konfigurasi](konfigurasi/data-master.md)

-   :material-play-circle-outline: **Operasional**

    ---

    Panduan menjalankan proses bisnis sehari-hari.

    [:octicons-arrow-right-24: Lihat Panduan](operasional/penugasan-karyawan.md)

-   :material-file-document-outline: **Contoh Kasus**

    ---

    Contoh nyata implementasi dari skenario perusahaan outsourcing.

    [:octicons-arrow-right-24: Lihat Contoh](contoh-kasus/skenario-dasar.md)

</div>

---

!!! tip "Rekomendasi Urutan Membaca"
    Bagi implementor yang baru memulai, disarankan membaca dokumentasi dengan urutan:
    
    1. [Gambaran Umum](konsep/gambaran-umum.md) → memahami sistem secara keseluruhan
    2. [Alur Bisnis](konsep/alur-bisnis.md) → memahami urutan proses
    3. [Konfigurasi Data Master](konfigurasi/data-master.md) → menyiapkan sistem
    4. [Contoh Kasus Dasar](contoh-kasus/skenario-dasar.md) → melihat implementasi nyata
