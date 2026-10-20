# ✅ Tasks — Daily Learning Report
## RevoU Coding Camp

**Versi:** 1.0  
**Tanggal:** 20 Oktober 2026  
**Referensi:** [REQUIREMENTS.md](./REQUIREMENTS.md) · [DESIGN.md](./DESIGN.md)

---

## Cara Menggunakan

- `[ ]` → Belum dikerjakan
- `[~]` → Sedang dikerjakan
- `[x]` → Selesai

---

## Phase 1 — Fondasi & Struktur HTML

> Membangun kerangka dasar file HTML beserta elemen-elemen utamanya.

- [ ] **TASK-01** — Buat file `daily-report.html` dengan struktur HTML5 dasar (`<!DOCTYPE>`, `<head>`, `<meta charset>`, `<meta viewport>`, `<title>`)
- [ ] **TASK-02** — Buat elemen `<header>` berisi brand icon, judul aplikasi, subtitle, dan area statistik (jumlah laporan & hari)
- [ ] **TASK-03** — Buat elemen `<main>` sebagai container utama konten
- [ ] **TASK-04** — Buat section form input dengan field: Tanggal (`date`), Topik (`text`), Rangkuman (`textarea`), Kendala & Solusi (`textarea`)
- [ ] **TASK-05** — Tambahkan tombol "Simpan Laporan" dan tombol "Export Markdown" di dalam form
- [ ] **TASK-06** — Buat section toolbar (label "Daftar Laporan" + search bar)
- [ ] **TASK-07** — Buat container `<div id="reportsList">` sebagai tempat render kartu laporan
- [ ] **TASK-08** — Buat elemen modal overlay untuk fitur Export Markdown (berisi textarea preview + tombol Salin & Unduh)
- [ ] **TASK-09** — Buat elemen toast notifikasi (`<div id="toast">`)

---

## Phase 2 — Styling & Tampilan (CSS)

> Mengimplementasikan desain visual dark mode yang responsif.

### 2.1 Setup & Base

- [ ] **TASK-10** — Definisikan CSS Variables (color tokens, radius, shadow, transition) di `:root`
- [ ] **TASK-11** — Reset CSS (`*, box-sizing: border-box`, margin/padding 0) dan styling `body` (font, background, color)
- [ ] **TASK-12** — Styling custom scrollbar (warna gelap, thumb ungu saat hover)

### 2.2 Header

- [ ] **TASK-13** — Styling header: `sticky`, backdrop blur, border bawah, layout flex space-between
- [ ] **TASK-14** — Styling brand icon (kotak gradasi ungu), teks judul, dan subtitle
- [ ] **TASK-15** — Styling area statistik (angka besar warna aksen, label kecil uppercase)

### 2.3 Form

- [ ] **TASK-16** — Styling form card (background, border, border-radius, box-shadow, padding)
- [ ] **TASK-17** — Styling CSS Grid 2 kolom untuk layout form, textarea full-width (`grid-column: 1 / -1`)
- [ ] **TASK-18** — Styling input dan textarea (dark background, border, focus state dengan glow ungu)
- [ ] **TASK-19** — Styling label form (uppercase, warna muted)
- [ ] **TASK-20** — Styling `input[type="date"]` agar calendar icon berwarna terang

### 2.4 Tombol

- [ ] **TASK-21** — Styling base `.btn` (flex, gap, padding, border-radius, font-weight, transisi)
- [ ] **TASK-22** — Styling variant tombol: `.btn-primary` (ungu), `.btn-danger` (merah outline), `.btn-edit` (ungu outline), `.btn-export` (hijau outline), `.btn-save-edit` (hijau solid)
- [ ] **TASK-23** — Tambahkan efek hover dan `scale(0.97)` saat tombol diklik (`:active`)

### 2.5 Toolbar & Search

- [ ] **TASK-24** — Styling toolbar: flex layout, label section kiri, search bar kanan
- [ ] **TASK-25** — Styling search bar dengan icon 🔍 posisi absolute di dalam input

### 2.6 Kartu Laporan

- [ ] **TASK-26** — Styling `.report-card` (background, border, padding, border-radius, shadow)
- [ ] **TASK-27** — Tambahkan animasi `@keyframes cardIn` (fade + slide up) saat kartu muncul
- [ ] **TASK-28** — Styling hover kartu (border berubah ungu, `translateY(-2px)`)
- [ ] **TASK-29** — Styling header kartu: layout flex, meta (tanggal + topik) di kiri, tombol aksi di kanan
- [ ] **TASK-30** — Styling section title dalam kartu (warna aksen, uppercase kecil) dan teks konten

### 2.7 Empty State

- [ ] **TASK-31** — Styling empty state (teks terpusat, icon besar, warna muted)

### 2.8 Modal Export

- [ ] **TASK-32** — Styling modal overlay (fixed inset, background gelap semi-transparan, backdrop blur)
- [ ] **TASK-33** — Styling modal box (background card, border, padding, border-radius, max-height, flex column)
- [ ] **TASK-34** — Styling `<textarea>` preview Markdown (font monospace, warna secondary, scroll)

### 2.9 Toast

- [ ] **TASK-35** — Styling toast (fixed bottom-right, animasi opacity + translateY, auto show/hide)

### 2.10 Responsivitas

- [ ] **TASK-36** — Tambahkan media query `@media (max-width: 640px)` untuk layout mobile
- [ ] **TASK-37** — Pada mobile: ubah form grid menjadi 1 kolom, kurangi padding header, kartu, dan modal

---

## Phase 3 — Logika Aplikasi (JavaScript)

> Mengimplementasikan semua fungsi interaktif.

### 3.1 Storage

- [ ] **TASK-38** — Buat fungsi `getReports()`: baca LocalStorage, parse JSON, return array (fallback `[]`)
- [ ] **TASK-39** — Buat fungsi `setReports(data)`: stringify dan simpan ke LocalStorage

### 3.2 Utilitas

- [ ] **TASK-40** — Buat fungsi `formatDate(str)`: konversi `YYYY-MM-DD` → format "Senin, 20 Oktober 2026"
- [ ] **TASK-41** — Buat fungsi `escapeHTML(str)`: sanitasi karakter `<`, `>`, `&`, `"` untuk keamanan XSS
- [ ] **TASK-42** — Buat fungsi `showToast(msg, icon)`: tampilkan toast, auto-hide setelah 2,8 detik
- [ ] **TASK-43** — Buat fungsi `updateStats()`: hitung total laporan & hari unik, update elemen header

### 3.3 Form Simpan

- [ ] **TASK-44** — Buat fungsi `saveReport()`: ambil nilai form → validasi wajib → buat objek → `unshift` ke array → simpan → reset form → render → toast
- [ ] **TASK-45** — Set nilai default `inputDate` ke tanggal hari ini saat halaman dimuat

### 3.4 Render Laporan

- [ ] **TASK-46** — Buat fungsi `renderReports()`: ambil data → filter berdasarkan query search → generate HTML kartu → inject ke DOM
- [ ] **TASK-47** — Tambahkan logika empty state: tampilkan pesan berbeda antara "belum ada laporan" dan "tidak ada hasil pencarian"

### 3.5 Edit Laporan

- [ ] **TASK-48** — Buat fungsi `editReport(id)`: ganti `innerHTML` kartu target dengan form edit inline yang terisi data lama
- [ ] **TASK-49** — Buat fungsi `saveEdit(id)`: validasi → update objek dalam array → simpan → render ulang → toast
- [ ] **TASK-50** — Tombol "Batal" pada form edit memanggil `renderReports()` untuk kembali ke tampilan kartu

### 3.6 Hapus Laporan

- [ ] **TASK-51** — Buat fungsi `deleteReport(id)`: tampilkan `confirm()` → filter array → simpan → render ulang → toast

### 3.7 Export Markdown

- [ ] **TASK-52** — Buat fungsi `generateMarkdown()`: buat string Markdown lengkap (header + loop laporan + footer) dalam urutan terlama ke terbaru
- [ ] **TASK-53** — Buat fungsi `openExportModal()`: generate MD → isi textarea → tampilkan modal
- [ ] **TASK-54** — Buat fungsi `closeExportModal(e)`: sembunyikan modal (bisa dipanggil dari tombol atau klik overlay)
- [ ] **TASK-55** — Buat fungsi `copyMarkdown()`: gunakan Clipboard API, fallback ke `execCommand('copy')`, tampilkan toast
- [ ] **TASK-56** — Buat fungsi `downloadMarkdown()`: buat `Blob` → buat URL → trigger `<a download>` → revoke URL → toast

### 3.8 Keyboard & Event

- [ ] **TASK-57** — Tambahkan event listener `keydown` untuk:
  - `Escape` → tutup modal export
  - `Ctrl+Enter` (saat fokus di dalam form) → panggil `saveReport()`
- [ ] **TASK-58** — Tambahkan event listener `oninput` pada search bar → panggil `renderReports()` secara real-time

### 3.9 Inisialisasi

- [ ] **TASK-59** — Tambahkan `DOMContentLoaded` listener untuk: set tanggal default + `renderReports()` + `updateStats()`

---

## Phase 4 — Pengujian & Verifikasi

> Memastikan semua fitur berjalan sesuai acceptance criteria dari REQUIREMENTS.md.

- [ ] **TASK-60** — Uji form: isi semua field → klik Simpan → kartu muncul di daftar
- [ ] **TASK-61** — Uji validasi: klik Simpan dengan field kosong → toast peringatan muncul, laporan tidak tersimpan
- [ ] **TASK-62** — Uji persistensi: simpan laporan → refresh halaman → data masih ada
- [ ] **TASK-63** — Uji edit: klik Edit → ubah data → klik Simpan Perubahan → kartu ter-update
- [ ] **TASK-64** — Uji hapus: klik Hapus → konfirmasi → kartu hilang dari daftar
- [ ] **TASK-65** — Uji search: ketik keyword → hanya kartu yang cocok yang tampil
- [ ] **TASK-66** — Uji empty state: hapus semua laporan → pesan empty muncul
- [ ] **TASK-67** — Uji export: klik Export Markdown → modal muncul → klik Salin → paste ke editor → format benar
- [ ] **TASK-68** — Uji unduh: klik Unduh .md → file `learning-report.md` terunduh
- [ ] **TASK-69** — Uji shortcut: `Ctrl+Enter` di dalam form → laporan tersimpan
- [ ] **TASK-70** — Uji responsif mobile: buka di DevTools ukuran 375px → layout 1 kolom, tidak ada overflow
- [ ] **TASK-71** — Uji responsif desktop: buka di lebar 1280px → layout 2 kolom, tampil rapi
- [ ] **TASK-72** — Cek console browser: tidak ada error saat penggunaan normal

---

## Phase 5 — Deploy ke GitHub Pages

- [ ] **TASK-73** — Pastikan file `daily-report.html` ada di root repository
- [ ] **TASK-74** — Commit semua file: `daily-report.html`, `REQUIREMENTS.md`, `DESIGN.md`, `TASKS.md`
- [ ] **TASK-75** — Push ke branch `main` di GitHub
- [ ] **TASK-76** — Aktifkan GitHub Pages: Settings → Pages → Source: `main` branch → root `/`
- [ ] **TASK-77** — Akses URL GitHub Pages dan verifikasi aplikasi berjalan dengan benar

---

## Ringkasan Progress

| Phase | Total Task | Selesai |
|---|---|---|
| Phase 1 — HTML | 9 | 0 |
| Phase 2 — CSS | 28 | 0 |
| Phase 3 — JavaScript | 22 | 0 |
| Phase 4 — Pengujian | 13 | 0 |
| Phase 5 — Deploy | 5 | 0 |
| **Total** | **77** | **0** |
