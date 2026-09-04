# Dashboard Monitoring KDMP — Pemanfaatan Lahan PLN

Dashboard monitoring **rencana pemanfaatan lahan PT PLN (Persero) oleh Koperasi
Desa/Kelurahan Merah Putih (KDMP)**, dikelola Divisi Umum & Aset Properti.

Data ditarik **langsung (live)** dari Google Sheets setiap halaman dibuka —
tidak ada salinan data yang disimpan di repositori ini.

## Isi

| Berkas | Fungsi |
|---|---|
| `index.html` | Struktur halaman, gaya, dan routing antar menu |
| `app.js` | KPI, grafik, tabel, modal detail, analisis otomatis, ekspor CSV |
| `gviz-fetch.js` | Pengambilan data live dari Google Sheets (gviz/JSONP) + normalisasi |
| `pln-logo.png` | Logo pada sidebar (opsional — bila tidak ada, tampil wordmark "PLN") |

## Menu

- **Dashboard** — KPI ringkas, komposisi persetujuan, status aset, jenis
  sertipikat, peringkat unit & kota/kabupaten, luas lahan diminta per unit,
  dan status komunikasi.
- **Data Permohonan** — tabel lengkap yang dapat diurutkan dan difilter; klik
  baris untuk melihat seluruh kolom dari sheet; tersedia unduh CSV.
- **Analisis** — narasi yang disusun otomatis dari data sesuai filter aktif,
  metrik kunci, dan butir perhatian.
- **Cetak Laporan** — cetak / simpan PDF tanpa sidebar dan baris filter.

Satu baris filter di atas (kelompok, unit, persetujuan, status aset, pencarian
bebas) mengendalikan seluruh tampilan sekaligus.

## Sumber data

- Spreadsheet: `1Fi9SP0DuOnHzRGg857MNgrV1iXXGmHrKfbCiOcsn9O8`
- Tab (gid): `1494639969` — *Data Koperasi Merah Putih*
- Header berada di baris 4, data mulai baris 5.

Agar dashboard dapat membaca data, sheet harus dibagikan sebagai
**"Anyone with the link — Viewer"**. Bila akses ditutup, dashboard menampilkan
pesan galat dan tidak menampilkan data apa pun.

### Pemetaan kolom

| Kolom | Isi |
|---|---|
| B | No urut |
| C | Satuan / instansi pengusul |
| D | Pihak pemohon |
| E | Lokasi aset |
| F | Kota/Kabupaten |
| G | Unit pemilik aset |
| H | Desa/Kelurahan |
| I–J | Link Google Maps & koordinat |
| K–L | Luas sesuai sertipikat & luas yang diminta (m²) |
| M–N | Jenis & nomor sertipikat |
| O | Status aset (IDLE / sedang digunakan) |
| P | Rencana pengembangan/pemanfaatan |
| Q–R | Surat permohonan Pemda & status komunikasi |
| S | Keterangan |
| T–U | Nilai aset per m² & estimasi nilai sewa |
| V | PIC BUMN di daerah |
| W | Persetujuan tindak lanjut (diisi Divisi GA) |
| X | Progres kontrak |

Bila urutan kolom di sheet berubah, sesuaikan objek `COL` pada `gviz-fetch.js`.

## Privasi

Repositori dan halaman ini bersifat **publik**. Nomor HP PIC pada kolom V
**disamarkan** secara default. Untuk menampilkannya penuh, ubah di `app.js`:

```js
const TAMPILKAN_NOMOR_PIC = true;
```

Perlu diingat: sheet sumber sendiri harus berstatus publik agar tarikan live
berfungsi, sehingga seluruh isinya tetap dapat diakses siapa pun yang memiliki
tautannya.

## Menjalankan secara lokal

Karena data diambil lewat JSONP, cukup layani berkas dengan server statis:

```bash
python3 -m http.server 8000
# lalu buka http://localhost:8000
```

## Pemeliharaan

- **Kolom baru di sheet** → tambahkan indeksnya pada `COL` dan tampilkan di
  `openModal()` / `exportCsv()` pada `app.js`.
- **Aturan pengelompokan unit** → `normKelompok()` di `gviz-fetch.js`.
- **Normalisasi status** → `normPersetujuan()`, `normStatusAset()`,
  `normJenisSertipikat()`, `normKomunikasi()` di `gviz-fetch.js`.
