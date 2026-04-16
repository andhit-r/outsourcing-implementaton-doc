# Copilot Instructions — outsourcing-implementation-doc

## Tujuan Repositori

Repositori ini berisi **dokumentasi implementasi** untuk pengelolaan penggajian karyawan outsource menggunakan modul-modul Odoo 14 yang dikembangkan oleh PT. Simetri Sinergi Indonesia (SSI / OpenSynergy Indonesia).

Dokumentasi ini ditujukan untuk **implementor** (bukan developer atau end-user), yaitu orang yang bertugas mengkonfigurasi dan mendeploykan sistem Odoo untuk perusahaan penyedia jasa outsourcing.

---

## Ruang Lingkup Modul yang Didokumentasikan

| Repositori | Modul Utama | Fungsi |
|---|---|---|
| `opnsynid-hr-payroll` | `ssi_hr_payroll` | Pemrosesan slip gaji dan entri akuntansi |
| `opnsynid-hr-payroll` | `ssi_hr_payroll_batch` | Pemrosesan slip gaji massal (batch) |
| `ssi-payroll-agreement` | `ssi_payroll_agreement` | Perjanjian penggajian per karyawan |
| `ssi-employee-external-assignment` | `ssi_employee_external_assignment` | Penugasan karyawan ke klien eksternal |
| `ssi-employee-external-assignment` | `ssi_employee_external_assignment_agreement` | Integrasi perjanjian untuk penugasan eksternal |

---

## Panduan Penulisan Dokumentasi

### Gaya Bahasa
- Gunakan **Bahasa Indonesia** yang formal dan mudah dipahami
- Hindari istilah teknis pemrograman (Python, ORM, dll.) kecuali jika sangat diperlukan
- Gunakan istilah bisnis yang umum dikenal implementor Odoo
- Sertakan **contoh nyata** (nama perusahaan fiktif, nama karyawan fiktif, angka gaji fiktif)

### Format Konten
- Ikuti format **MkDocs Material** dengan frontmatter YAML
- Setiap halaman harus memiliki heading yang jelas dan terstruktur
- Gunakan **admonition** (note, warning, tip, example) untuk menyoroti informasi penting
- Sertakan **diagram alur** menggunakan Mermaid untuk menggambarkan proses bisnis
- Gunakan **tabel** untuk perbandingan dan daftar konfigurasi

### Jenis Konten yang Perlu Ada
1. **Konsep** — Penjelasan konseptual tentang bagaimana fitur bekerja
2. **Konfigurasi** — Langkah-langkah mengatur master data dan konfigurasi sistem
3. **Operasional** — Cara menjalankan proses bisnis sehari-hari
4. **Contoh Kasus** — Skenario nyata dari perusahaan outsourcing

### Yang TIDAK Perlu Ada
- Kode Python atau konfigurasi teknis
- Instruksi instalasi modul (bukan scope dokumentasi ini)
- Penjelasan arsitektur teknis

---

## Struktur Direktori Dokumentasi

```
docs/
├── index.md                      # Halaman utama / pengantar
├── konsep/
│   ├── gambaran-umum.md          # Overview sistem
│   ├── alur-bisnis.md            # Alur bisnis end-to-end
│   └── struktur-gaji.md          # Konsep struktur dan komponen gaji
├── konfigurasi/
│   ├── data-master.md            # Setup data master
│   ├── struktur-gaji.md          # Konfigurasi struktur gaji
│   ├── tipe-penugasan.md         # Konfigurasi tipe penugasan
│   └── tipe-perjanjian.md        # Konfigurasi tipe perjanjian
├── operasional/
│   ├── penugasan-karyawan.md     # Proses penugasan karyawan ke klien
│   ├── perjanjian-gaji.md        # Pembuatan perjanjian penggajian
│   ├── pemrosesan-gaji.md        # Proses pembuatan slip gaji
│   ├── batch-gaji.md             # Pemrosesan gaji massal
│   └── invoicing-klien.md        # Penagihan ke klien
└── contoh-kasus/
    ├── skenario-dasar.md          # Skenario sederhana
    └── skenario-lanjutan.md       # Skenario kompleks multi-klien
```

---

## Konvensi Nama

- Gunakan nama perusahaan fiktif: **PT. Maju Bersama** (perusahaan outsourcing/vendor)
- Gunakan nama klien fiktif: **PT. Karya Utama**, **PT. Nusantara Jaya**, **CV. Berkah Abadi**
- Gunakan nama karyawan fiktif: **Budi Santoso**, **Sari Dewi**, **Ahmad Fauzi**
- Nilai gaji dalam Rupiah (IDR) dengan format yang wajar

---

## Deploy ke GitHub Pages

Dokumentasi di-deploy ke GitHub Pages menggunakan branch `master`. File konfigurasi MkDocs ada di `mkdocs.yml` di root repositori.
