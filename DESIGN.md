# 🎨 Design Document
## Daily Learning Report — RevoU Coding Camp

**Versi:** 1.0  
**Tanggal:** 20 Oktober 2026  
**Referensi:** [REQUIREMENTS.md](./REQUIREMENTS.md)

---

## 1. Arsitektur Aplikasi

Aplikasi ini sepenuhnya berjalan di sisi klien (client-side) tanpa memerlukan server atau backend.

```
┌─────────────────────────────────────────────┐
│                Browser (Client)             │
│                                             │
│  ┌──────────┐   ┌──────────┐  ┌──────────┐ │
│  │   HTML   │   │   CSS    │  │    JS    │ │
│  │(Struktur)│   │(Tampilan)│  │ (Logika) │ │
│  └──────────┘   └──────────┘  └────┬─────┘ │
│                                    │        │
│                          ┌─────────▼──────┐ │
│                          │  LocalStorage  │ │
│                          │ (Penyimpanan)  │ │
│                          └────────────────┘ │
└─────────────────────────────────────────────┘
```

### Pola Arsitektur: Event-Driven Vanilla JS

Tidak menggunakan framework. Semua interaksi dikelola melalui:
- **Event listeners** pada elemen DOM
- **State** langsung dari LocalStorage (sumber kebenaran tunggal)
- **Render function** yang menulis ulang tampilan setiap kali data berubah

---

## 2. Struktur Data

### 2.1 Schema Objek Laporan

Setiap laporan disimpan sebagai objek JavaScript dengan struktur berikut:

```js
{
  id:        String,   // timestamp, cth: "1729382400000"
  date:      String,   // format ISO date: "YYYY-MM-DD"
  topic:     String,   // nama materi/topik
  summary:   String,   // rangkuman pembelajaran (bisa multiline)
  obstacle:  String,   // kendala dan solusi (bisa multiline)
  createdAt: String    // ISO datetime: "2026-10-20T08:00:00.000Z"
}
```

### 2.2 Format Penyimpanan LocalStorage

```js
// Key
"revou_daily_reports"

// Value (JSON string dari array objek laporan)
[
  { id: "...", date: "...", topic: "...", summary: "...", obstacle: "...", createdAt: "..." },
  { id: "...", date: "...", topic: "...", ... }
]
```

> Array diurutkan dari **terbaru ke terlama** (index 0 = laporan paling baru).

---

## 3. Komponen UI

Aplikasi terdiri dari empat komponen utama yang dirender secara berurutan dari atas ke bawah.

```
┌──────────────────────────────────────┐
│             [HEADER]                 │  ← sticky, z-index: 100
│   Logo | Judul | Statistik (total)   │
├──────────────────────────────────────┤
│           [FORM CARD]                │  ← input laporan baru
│  Tanggal | Topik                     │
│  Rangkuman (textarea)                │
│  Kendala & Solusi (textarea)         │
│  [Simpan] [Export Markdown]          │
├──────────────────────────────────────┤
│  [TOOLBAR]  Label | [Search Bar]     │  ← filter real-time
├──────────────────────────────────────┤
│           [REPORT FEED]              │  ← daftar kartu
│  ┌────────────────────────────────┐  │
│  │ CARD: Tanggal · Topik          │  │
│  │ [Edit] [Hapus]                 │  │
│  │ ── Rangkuman ──────────────── │  │
│  │ ── Kendala & Solusi ────────── │  │
│  └────────────────────────────────┘  │
│  ┌────────────────────────────────┐  │
│  │ CARD: ...                      │  │
│  └────────────────────────────────┘  │
└──────────────────────────────────────┘

[ MODAL EXPORT ] ← overlay, muncul saat klik Export
[ TOAST ]        ← notifikasi pojok kanan bawah
```

### 3.1 Header

| Elemen | Detail |
|---|---|
| Brand icon | Emoji 📓 dalam kotak gradasi ungu |
| Judul | "Daily Learning Report" + subtitle "RevoU Coding Camp" |
| Statistik | Counter jumlah laporan + jumlah hari unik |
| Posisi | `sticky top: 0` agar selalu terlihat saat scroll |

### 3.2 Form Card

| Elemen | Tipe HTML | Validasi |
|---|---|---|
| Tanggal | `<input type="date">` | Wajib diisi |
| Topik | `<input type="text">` | Wajib diisi |
| Rangkuman | `<textarea>` | Opsional |
| Kendala & Solusi | `<textarea>` | Opsional |
| Simpan | `<button>` | Trigger validasi |
| Export Markdown | `<button>` | Buka modal |

Layout menggunakan **CSS Grid 2 kolom** — Tanggal dan Topik berdampingan, sedangkan kedua textarea melebar penuh (`grid-column: 1 / -1`).

### 3.3 Report Card

Setiap kartu menampilkan satu laporan dengan dua state:

**State: View (default)**
```
┌─────────────────────────────────────────┐
│ 📅 Senin, 20 Oktober 2026  · #1         │
│ Nama Topik / Materi                     │
│                      [✏️ Edit] [🗑️]     │
│ ─────────────────────────────────────── │
│ 📝 RANGKUMAN                            │
│ Teks rangkuman...                       │
│ ─────────────────────────────────────── │
│ 🚧 KENDALA & SOLUSI                     │
│ Teks kendala...                         │
└─────────────────────────────────────────┘
```

**State: Edit (inline)**
```
┌─────────────────────────────────────────┐
│ [input: Tanggal]                        │
│ [input: Topik]                          │
│ [textarea: Rangkuman]                   │
│ [textarea: Kendala]                     │
│ [✅ Simpan Perubahan] [✕ Batal]         │
└─────────────────────────────────────────┘
```

### 3.4 Modal Export

- Overlay penuh layar dengan backdrop blur
- Textarea read-only berisi teks Markdown yang di-generate
- Tombol: Salin Semua, Unduh .md, Tutup
- Ditutup dengan klik di luar modal atau tombol Tutup atau tekan `Esc`

### 3.5 Toast Notification

- Posisi: `fixed bottom-right`
- Tampil selama **2,8 detik** lalu fade out
- Berisi icon + pesan singkat
- Digunakan untuk: simpan, hapus, edit, salin, unduh

---

## 4. Desain Visual

### 4.1 Color Palette (Dark Mode)

| Token | Hex | Penggunaan |
|---|---|---|
| `--bg-primary` | `#0f1117` | Background utama halaman |
| `--bg-card` | `#1a1d27` | Background kartu & form |
| `--bg-input` | `#13151f` | Background input field |
| `--border` | `#2a2d3e` | Garis border elemen |
| `--accent` | `#7c6af7` | Warna utama (ungu) — CTA, highlight |
| `--accent-hover` | `#6355e0` | Hover state aksen |
| `--accent-soft` | `rgba(124,106,247,0.12)` | Background lunak aksen |
| `--danger` | `#e05252` | Tombol hapus |
| `--success` | `#43c97e` | Tombol export & simpan edit |
| `--text-primary` | `#e8eaf6` | Teks utama |
| `--text-secondary` | `#8b8fa8` | Teks konten kartu |
| `--text-muted` | `#555870` | Label, metadata |

### 4.2 Tipografi

| Elemen | Ukuran | Weight |
|---|---|---|
| Brand title | `1.1rem` | 700 |
| Section label | `0.72rem` uppercase | 600 |
| Form label | `0.78rem` | 600 |
| Input / textarea | `0.9rem` | 400 |
| Topik kartu | `1rem` | 700 |
| Teks konten kartu | `0.88rem` | 400 |
| Tombol | `0.85rem` | 600 |

**Font stack:** `'Segoe UI', system-ui, -apple-system, sans-serif`  
**Font kode (modal):** `'Cascadia Code', 'Fira Code', 'Consolas', monospace`

### 4.3 Spacing & Radius

| Token | Nilai | Digunakan untuk |
|---|---|---|
| `--radius-sm` | `8px` | Input, tombol, toast |
| `--radius-md` | `12px` | — (cadangan) |
| `--radius-lg` | `16px` | Kartu, form, modal |
| Gap antar kartu | `16px` | Report feed |
| Padding kartu | `24px` | Desktop |
| Padding kartu | `18px 16px` | Mobile |

### 4.4 Efek & Animasi

| Elemen | Efek |
|---|---|
| Kartu baru muncul | `@keyframes cardIn` — fade + slide up (0.3s) |
| Kartu saat hover | `translateY(-2px)` + border ungu |
| Tombol saat klik | `scale(0.97)` |
| Input saat focus | Border ungu + `box-shadow` glow |
| Toast masuk/keluar | `opacity` + `translateY` (0.25s) |
| Semua transisi | `0.2s ease` |

---

## 5. Logika Aplikasi (JavaScript)

### 5.1 Fungsi Utama

| Fungsi | Tanggung Jawab |
|---|---|
| `getReports()` | Membaca & mem-parse data dari LocalStorage |
| `setReports(data)` | Menyimpan array laporan ke LocalStorage |
| `saveReport()` | Validasi form → buat objek → simpan → render ulang |
| `deleteReport(id)` | Konfirmasi → filter array → simpan → render ulang |
| `editReport(id)` | Ganti konten kartu dengan form edit inline |
| `saveEdit(id)` | Validasi → update objek di array → simpan → render ulang |
| `renderReports()` | Generate HTML kartu dari data, termasuk filter search |
| `updateStats()` | Hitung total laporan & hari unik → update header |
| `generateMarkdown()` | Convert semua laporan ke teks format Markdown |
| `openExportModal()` | Generate MD → tampilkan modal |
| `closeExportModal()` | Sembunyikan modal |
| `copyMarkdown()` | Clipboard API → fallback `execCommand` |
| `downloadMarkdown()` | Buat Blob → trigger download via `<a>` |
| `showToast(msg, icon)` | Tampilkan notifikasi, auto-hide setelah 2,8 detik |
| `formatDate(str)` | Konversi `YYYY-MM-DD` → "Senin, 20 Oktober 2026" |
| `escapeHTML(str)` | Sanitasi string sebelum di-render ke DOM |

### 5.2 Alur Simpan Laporan

```
Pengguna klik [Simpan]
       │
       ▼
Validasi: date && topic ada?
       │
   ┌───┴─────┐
  Tidak      Ya
   │          │
   ▼          ▼
Toast ⚠️   Buat objek laporan
            │
            ▼
         unshift ke array
            │
            ▼
         setReports() → LocalStorage
            │
            ▼
         Reset form
            │
            ▼
         renderReports()
            │
            ▼
         updateStats()
            │
            ▼
         Toast ✅
```

### 5.3 Alur Edit Laporan

```
Klik [Edit] pada kartu
       │
       ▼
editReport(id) → ganti innerHTML kartu dengan form edit
       │
       ▼
Pengguna ubah data → klik [Simpan Perubahan]
       │
       ▼
saveEdit(id) → validasi → map array → update objek
       │
       ▼
setReports() → renderReports() → Toast ✏️
```

### 5.4 Alur Export Markdown

```
Klik [Export Markdown]
       │
       ▼
generateMarkdown()
→ header + loop laporan (dibalik, terlama ke terbaru) + footer
       │
       ▼
Isi <textarea #mdOutput>
       │
       ▼
Tampilkan modal
       │
  ┌────┴────┐
  ▼         ▼
[Salin]  [Unduh .md]
  │         │
  ▼         ▼
Clipboard  Blob → <a download>
```

---

## 6. Responsivitas

### Breakpoint

| Kondisi | Lebar layar | Perubahan layout |
|---|---|---|
| Desktop (default) | ≥ 641px | Form grid 2 kolom, padding penuh |
| Mobile | ≤ 640px | Form grid 1 kolom, padding dikecilkan |

### Perubahan pada Mobile (≤ 640px)

- `header` padding: `20px 24px` → `14px 16px`
- `main` padding: `32px 24px 80px` → `20px 16px 60px`
- `.form-grid` dari 2 kolom → 1 kolom
- `.form-card` padding: `28px` → `20px 16px`
- `.report-card` padding: `24px` → `18px 16px`
- `.modal` padding: `28px` → `20px 16px`
- Stat number font size: `1.4rem` → `1.1rem`

---

## 7. Keamanan & Sanitasi

| Risiko | Mitigasi |
|---|---|
| XSS via input pengguna | Semua teks di-escape dengan fungsi `escapeHTML()` sebelum di-render ke innerHTML |
| Corrupt data di LocalStorage | `getReports()` dibungkus try-catch, fallback ke array kosong |
| Clipboard API tidak didukung | Fallback ke `document.execCommand('copy')` |

---

## 8. Struktur File Final

```
revoyou-coding-cam-1-/
├── daily-report.html   ← aplikasi utama (HTML + CSS + JS)
├── REQUIREMENTS.md     ← dokumen kebutuhan
├── DESIGN.md           ← dokumen ini
└── README.md           ← portofolio (diisi dari hasil export)
```

---

## 9. Keputusan Desain & Pertimbangannya

| Keputusan | Alasan |
|---|---|
| Single-file HTML | Mudah di-deploy ke GitHub Pages tanpa konfigurasi build |
| Vanilla JS tanpa framework | Menghindari dependensi eksternal, ukuran file kecil |
| LocalStorage sebagai storage | Tidak butuh backend, cukup untuk kebutuhan personal |
| CSS Variables untuk tema | Konsistensi visual, mudah diubah jika ingin ganti tema |
| Inline edit (bukan modal) | Lebih cepat, tidak perlu buka/tutup popup untuk edit |
| Render ulang penuh setiap perubahan | Simpel dan konsisten, tidak ada state yang bisa out-of-sync |
| Markdown diurutkan terlama ke terbaru | Lebih natural dibaca sebagai jurnal progres di README |
