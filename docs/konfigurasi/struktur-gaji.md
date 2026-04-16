---
title: Konfigurasi Struktur Gaji
description: Panduan lengkap mengkonfigurasi struktur dan komponen gaji untuk perusahaan outsourcing
---

# Konfigurasi Struktur Gaji

Halaman ini menjelaskan cara mengkonfigurasi struktur gaji secara lengkap, mulai dari membuat komponen hingga merakit struktur siap pakai.

---

## Prinsip Konfigurasi

Sebelum mulai konfigurasi, implementor harus sudah menjawab pertanyaan berikut:

1. **Ada berapa jenis posisi/pekerjaan yang perlu skema gaji berbeda?**  
   Contoh: Operator Produksi, Staf Admin, Satpam — berarti butuh 3 struktur gaji.

2. **Apa saja komponen gaji yang berlaku untuk SEMUA karyawan?**  
   Komponen ini masuk ke struktur induk.

3. **Apa saja komponen yang HANYA berlaku untuk posisi tertentu?**  
   Komponen ini masuk ke struktur masing-masing posisi.

4. **Komponen mana yang nilainya SAMA untuk semua karyawan?**  
   Nilainya dikonfigurasi di rule.

5. **Komponen mana yang nilainya BERBEDA per karyawan?**  
   Nilainya dikonfigurasi melalui input di perjanjian gaji.

---

## Langkah 1 — Buat Komponen Gaji Penghasilan

### Gaji Pokok

Gaji pokok biasanya nilainya berbeda per karyawan, sehingga menggunakan **input dari perjanjian gaji**.

**Menu:** `Penggajian > Konfigurasi > Komponen Gaji > Baru`

| Field | Nilai | Keterangan |
|---|---|---|
| Nama | `Gaji Pokok` | |
| Kode | `BASIC` | Digunakan sebagai referensi di komponen lain |
| Kategori | `Penghasilan Kotor` | |
| Urutan | `10` | Tampil paling atas |
| Kondisi | Selalu aktif | |
| Cara Hitung | Dari input | Gunakan input bertipe `Gaji Pokok` |

### Tunjangan Transportasi

| Field | Nilai |
|---|---|
| Nama | `Tunjangan Transportasi` |
| Kode | `TRANS` |
| Kategori | `Penghasilan Kotor` |
| Urutan | `20` |
| Cara Hitung | Dari input perjanjian bertipe `Tunjangan Transportasi` |

!!! tip "Filosofi Desain"
    Semua tunjangan yang nilainya bisa berbeda per karyawan **sebaiknya menggunakan input**, bukan nilai tetap di komponen. Ini memudahkan saat ada karyawan dengan tunjangan berbeda.

---

## Langkah 2 — Buat Komponen Potongan Wajib

### BPJS Kesehatan Karyawan (1%)

| Field | Nilai |
|---|---|
| Nama | `BPJS Kesehatan Karyawan` |
| Kode | `BPJS_KES_EE` |
| Kategori | `Potongan` |
| Urutan | `100` |
| Cara Hitung | `1% × nilai komponen BASIC` |
| Akun Kredit | Akun utang BPJS Kesehatan |

### BPJS Kesehatan Perusahaan (4%)

| Field | Nilai |
|---|---|
| Nama | `BPJS Kesehatan Perusahaan` |
| Kode | `BPJS_KES_ER` |
| Kategori | `Iuran Perusahaan` |
| Urutan | `110` |
| Cara Hitung | `4% × nilai komponen BASIC` |

### BPJS TK — JHT Karyawan (2%)

| Field | Nilai |
|---|---|
| Nama | `BPJS TK JHT Karyawan` |
| Kode | `BPJS_TK_JHT_EE` |
| Kategori | `Potongan` |
| Urutan | `120` |
| Cara Hitung | `2% × nilai komponen BASIC` |

### BPJS TK — JHT Perusahaan (3.7%)

| Field | Nilai |
|---|---|
| Nama | `BPJS TK JHT Perusahaan` |
| Kode | `BPJS_TK_JHT_ER` |
| Kategori | `Iuran Perusahaan` |
| Urutan | `130` |
| Cara Hitung | `3.7% × nilai komponen BASIC` |

---

## Langkah 3 — Buat Komponen Gaji Bersih

Komponen ini menghitung total akhir yang diterima karyawan.

| Field | Nilai |
|---|---|
| Nama | `Gaji Bersih` |
| Kode | `NET` |
| Kategori | `Gaji Bersih` |
| Urutan | `999` |
| Cara Hitung | `Total Kategori Gross − Total Kategori Potongan` |

---

## Langkah 4 — Buat Struktur Gaji Induk

**Menu:** `Penggajian > Konfigurasi > Struktur Gaji > Baru`

| Field | Nilai |
|---|---|
| Nama | `Struktur Gaji Dasar Outsource` |
| Induk | (kosong) |

**Komponen yang dimasukkan ke struktur induk:**

- Gaji Pokok
- BPJS Kesehatan Karyawan
- BPJS Kesehatan Perusahaan
- BPJS TK JHT Karyawan
- BPJS TK JHT Perusahaan
- Gaji Bersih

---

## Langkah 5 — Buat Struktur Gaji per Posisi

### Struktur: Gaji Operator Produksi

| Field | Nilai |
|---|---|
| Nama | `Gaji Operator Produksi` |
| Induk | `Struktur Gaji Dasar Outsource` |

**Komponen tambahan (di luar yang diwarisi dari induk):**

- Tunjangan Transportasi
- Tunjangan Makan

### Struktur: Gaji Staf Administrasi

| Field | Nilai |
|---|---|
| Nama | `Gaji Staf Administrasi` |
| Induk | `Struktur Gaji Dasar Outsource` |

**Komponen tambahan:**

- Tunjangan Komunikasi

### Struktur: Gaji Satpam

| Field | Nilai |
|---|---|
| Nama | `Gaji Satpam` |
| Induk | `Struktur Gaji Dasar Outsource` |

**Komponen tambahan:**

- Tunjangan Shift Malam (hanya aktif jika input shift malam > 0)

---

## Contoh Hasil Konfigurasi

Berikut adalah contoh lengkap komponen yang akan muncul di slip gaji **Operator Produksi**:

| # | Komponen | Dari Struktur | Nilai |
|---|---|---|---|
| 1 | Gaji Pokok | Induk | Per perjanjian karyawan |
| 2 | Tunjangan Transportasi | Operator Produksi | Per perjanjian karyawan |
| 3 | Tunjangan Makan | Operator Produksi | Per perjanjian karyawan |
| 4 | BPJS Kesehatan Karyawan | Induk | 1% × Gaji Pokok |
| 5 | BPJS TK JHT Karyawan | Induk | 2% × Gaji Pokok |
| 6 | Gaji Bersih | Induk | (1+2+3) − (4+5) |

---

!!! warning "Perhatian: Urutan Komponen"
    Urutan (sequence) komponen gaji sangat penting. Komponen yang menjadi **dasar perhitungan** komponen lain harus memiliki nomor urut yang **lebih kecil** (tampil lebih dulu).
    
    Contoh yang benar:
    - `BASIC` (urutan 10) → harus lebih dulu dari
    - `BPJS_KES_EE` (urutan 100) → yang menghitung berdasarkan BASIC
