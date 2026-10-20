# 📋 Requirements Document
## Daily Learning Report — RevoU Coding Camp

**Versi:** 1.0  
**Tanggal:** 20 Oktober 2026  
**Platform:** Web (Single-file HTML, client-side only)

---

## 1. Latar Belakang

Peserta RevoU Coding Camp membutuhkan cara yang mudah dan cepat untuk mendokumentasikan progres belajar harian. Dokumentasi ini berguna untuk refleksi diri, portofolio GitHub, dan laporan ke mentor. Aplikasi ini dibuat sebagai single-file HTML agar dapat langsung di-deploy ke GitHub Pages tanpa backend.

---

## 2. Tujuan

- Menyediakan form input laporan harian yang sederhana.
- Menyimpan data laporan secara lokal di browser (tanpa server).
- Menampilkan riwayat laporan dalam bentuk kartu yang mudah dibaca.
- Memudahkan ekspor laporan ke format Markdown untuk README GitHub.

---

## 3. Pengguna (User)

| Persona | Deskripsi |
|---|---|
| **Peserta Bootcamp** | Pengguna utama yang mengisi laporan belajar harian |
| **Mentor / Reviewer** | Melihat laporan melalui README.md di GitHub (tidak langsung pakai app) |

---

## 4. Functional Requirements

### 4.1 Form Input Laporan

| ID | Requirement |
|---|---|
| FR-01 | Sistem menampilkan field input **Tanggal** bertipe `date`, otomatis terisi hari ini |
| FR-02 | Sistem menampilkan field input **Materi / Topik Pelajaran** bertipe teks |
| FR-03 | Sistem menampilkan textarea **Rangkuman Pembelajaran** untuk deskripsi panjang |
| FR-04 | Sistem menampilkan textarea **Kendala & Solusi** untuk mencatat hambatan |
| FR-05 | Tombol **Simpan Laporan** memvalidasi field wajib (Tanggal & Topik) sebelum menyimpan |
| FR-06 | Setelah menyimpan, form direset ke kondisi awal (tanggal kembali ke hari ini) |
| FR-07 | Pengguna dapat menekan `Ctrl+Enter` sebagai shortcut menyimpan laporan |

### 4.2 Penyimpanan Data

| ID | Requirement |
|---|---|
| FR-08 | Data laporan disimpan ke **LocalStorage** browser dengan key `revou_daily_reports` |
| FR-09 | Data tetap tersimpan saat halaman di-refresh atau browser ditutup dan dibuka kembali |
| FR-10 | Setiap laporan memiliki ID unik (timestamp) agar dapat diidentifikasi secara individual |

### 4.3 Daftar Laporan (Report Feed)

| ID | Requirement |
|---|---|
| FR-11 | Sistem menampilkan semua laporan tersimpan dalam bentuk **kartu (card)** |
| FR-12 | Laporan terbaru ditampilkan paling atas (urutan descending berdasarkan waktu simpan) |
| FR-13 | Setiap kartu menampilkan: tanggal, topik, rangkuman, dan kendala & solusi |
| FR-14 | Setiap kartu memiliki tombol **Edit** untuk mengubah isi laporan secara inline |
| FR-15 | Setiap kartu memiliki tombol **Hapus** dengan konfirmasi dialog sebelum menghapus |
| FR-16 | Terdapat **search bar** untuk memfilter laporan berdasarkan topik, rangkuman, atau tanggal |
| FR-17 | Jika belum ada laporan, ditampilkan **empty state** yang informatif |

### 4.4 Export Markdown

| ID | Requirement |
|---|---|
| FR-18 | Tombol **Export Markdown** membuka modal berisi pratinjau teks format `.md` |
| FR-19 | Markdown yang dihasilkan mencakup seluruh laporan tersimpan, diurutkan dari terlama ke terbaru |
| FR-20 | Tersedia tombol **Salin Semua** untuk menyalin teks Markdown ke clipboard |
| FR-21 | Tersedia tombol **Unduh .md** untuk mengunduh file `learning-report.md` langsung ke perangkat |
| FR-22 | Format Markdown kompatibel dengan GitHub README (menggunakan heading `##`, bold, dan `---`) |

---

## 5. Non-Functional Requirements

| ID | Requirement |
|---|---|
| NFR-01 | **Single-file:** Seluruh HTML, CSS, dan JavaScript berada dalam satu file `.html` |
| NFR-02 | **Tanpa dependensi eksternal:** Tidak membutuhkan library, CDN, atau koneksi internet untuk berjalan |
| NFR-03 | **Responsif:** Tampilan menyesuaikan layar HP (≥ 320px) dan laptop (≥ 960px) |
| NFR-04 | **Dark mode:** Menggunakan tema gelap secara default dengan kontras yang nyaman dibaca |
| NFR-05 | **Client-side only:** Tidak ada backend, database, atau API call — semua berjalan di browser |
| NFR-06 | **Deploy-ready:** File dapat langsung dihosting di GitHub Pages tanpa konfigurasi tambahan |
| NFR-07 | **Feedback instan:** Setiap aksi (simpan, hapus, salin) menampilkan notifikasi toast |
| NFR-08 | **Aksesibilitas dasar:** Label pada semua form input tersedia untuk screen reader |

---

## 6. Batasan Sistem

- Data hanya tersimpan di browser lokal — tidak tersinkronisasi antar perangkat.
- Tidak ada autentikasi atau multi-user; semua data bersifat personal per browser.
- Kapasitas penyimpanan terbatas oleh kuota LocalStorage (~5MB per origin).
- Ekspor Markdown hanya didukung di browser modern (Chrome, Firefox, Edge, Safari).

---

## 7. User Stories

```
Sebagai peserta bootcamp,
Saya ingin mengisi laporan belajar harian dengan mudah,
Agar progres saya terdokumentasi dengan rapi setiap hari.

Sebagai peserta bootcamp,
Saya ingin data laporan tidak hilang saat refresh halaman,
Agar saya tidak perlu mengisi ulang dari awal.

Sebagai peserta bootcamp,
Saya ingin bisa mengedit atau menghapus laporan yang sudah dibuat,
Agar saya bisa memperbaiki kesalahan input.

Sebagai peserta bootcamp,
Saya ingin mengekspor seluruh laporan ke format Markdown,
Agar bisa langsung ditempelkan ke README.md GitHub sebagai portofolio.

Sebagai peserta bootcamp,
Saya ingin mencari laporan berdasarkan topik atau tanggal,
Agar mudah menemukan catatan dari hari tertentu.
```

---

## 8. Struktur File

```
revoyou-coding-cam-1-/
├── daily-report.html     ← single-file app (HTML + CSS + JS)
├── REQUIREMENTS.md       ← dokumen ini
└── README.md             ← target tempel hasil export Markdown
```

---

## 9. Kriteria Penerimaan (Acceptance Criteria)

- [ ] Form dapat diisi dan laporan tersimpan tanpa error
- [ ] Data laporan tetap ada setelah halaman di-refresh
- [ ] Laporan dapat diedit secara inline dan perubahan tersimpan
- [ ] Laporan dapat dihapus setelah konfirmasi
- [ ] Search bar memfilter kartu secara real-time
- [ ] Export menghasilkan Markdown yang valid dan bisa disalin/diunduh
- [ ] Tampilan rapi di layar 375px (iPhone) dan 1280px (laptop)
- [ ] Tidak ada error di console browser saat digunakan normal
