# SIM Keripik Pisang Banaho - Khas Magetan 🍌

Dashboard Sistem Informasi Manajemen (SIM) Produsen & Reseller Keripik Pisang **Banaho** (Oleh-oleh Khas Magetan, Jawa Timur). Dirancang khusus sebagai *Single Page Application* (SPA) & *Progressive Web App* (PWA) mandiri berbasis HTML, Tailwind CSS, Chart.js, dan PapaParse yang siap di-deploy secara gratis melalui **GitHub Pages**.

---

## ⚡ Sistem Pengisian Data Online & Offline (Dual-Mode Sync)

Dashboard ini kini dilengkapi dengan arsitektur **Offline-First**, memungkinkan pencatatan transaksi di lokasi tanpa sinyal (misalnya di dapur penggorengan, kebun pisang, atau kios wisata lereng Gunung Lawu / Telaga Sarangan) tanpa khawatir kehilangan data:

### 1. Cara Kerja Mode Offline (Tanpa Jaringan Internet)
- **Deteksi Otomatis:** Sistem secara realtime mendeteksi status koneksi internet browser (`navigator.onLine`).
- **Pencatatan Tetap Berjalan:** Saat offline, tombol `+ Catat Penjualan`, log produksi, maupun penyesuaian stok tetap berfungsi normal 100%.
- **Penyimpanan Lokal Aman:** Data transaksi yang dicatat disimpan ke antrean offline (*Offline Queue*) di memori lokal perangkat (`localStorage`).
- **Langsung Masuk ke Visualisasi:** Transaksi offline langsung diperhitungkan di kartu KPI omzet, grafik, dan tabel rekap dengan tanda khusus:  
  `<span class="badge">⚡ Offline</span>` (Menunggu Sinkronisasi).

### 2. Cara Kerja Saat Koneksi Internet Tersambung Kembali
Ketika perangkat mendeteksi jaringan internet aktif kembali:
1. Indikator jaringan di pojok kanan atas otomatis beralih menjadi `🟢 Online`.
2. Notifikasi pop-up muncul: *"Koneksi internet aktif! Terdapat X data offline siap disinkronkan."*
3. Tombol antrean sinkronisasi `⚡ X Antrean Offline` akan berkedip mengajak pengguna melakukan sinkronisasi.

### 3. Tiga (3) Opsi Sinkronisasi ke Google Sheets:

Buka **Pusat Antrean Sinkronisasi** (klik tombol antrean berkedip di header atau menu sidebar):

| Metode Sinkronisasi | Deskripsi & Kelebihan |
| :--- | :--- |
| **🚀 Kirim Otomatis ke Cloud (Google Apps Script)** | Sistem otomatis mengirim seluruh data offline satu per satu langsung ke baris Google Sheets via Web App API tanpa perlu copy-paste. |
| **📋 Salin Format Baris (1-Klik Copy)** | **Paling Praktis Tanpa Koding:** Sistem membuat teks format *Tab-Separated Values* (TSV) yang persis dengan urutan kolom Google Sheets Anda. Cukup klik tombol ini, buka Google Sheets, lalu tekan **Ctrl+V** (Cmd+V) pada baris baru! |
| **💾 Unduh CSV Data Offline** | Mengunduh file CSV khusus berisi transaksi yang dicatat saat offline untuk arsip atau impor manual. |

---

## 📜 Panduan Setup Google Apps Script (Untuk Sinkron Otomatis)

Jika Anda ingin data offline/online terkirim otomatis ke Google Sheets secara realtime:

1. Buka spreadsheet Google Sheets Anda.
2. Klik menu **Ekstensi (Extensions)** &rarr; **Apps Script**.
3. Hapus kode bawaan dan tempelkan script berikut:
   ```javascript
   function doPost(e) {
     var sheet = SpreadsheetApp.getActiveSpreadsheet().getSheetByName("Penjualan");
     var data = JSON.parse(e.postData.contents);
     sheet.appendRow([
       data.tanggal, 
       data.mitra, 
       data.kategori, 
       data.varian, 
       data.jumlah, 
       data.satuan, 
       data.hargaSatuan, 
       data.total, 
       data.status
     ]);
     return ContentService.createTextOutput(JSON.stringify({result: "success"}))
       .setMimeType(ContentService.MimeType.JSON);
   }
   ```
4. Klik tombol **Terapkan (Deploy)** di pojok kanan atas &rarr; **Penerapan Baru (New Deployment)**.
5. Pilih jenis: **Aplikasi Web (Web App)**.
   - *Jalankan sebagai (Execute as)*: **Saya (Me)**
   - *Siapa yang memiliki akses (Who has access)*: **Siapa saja (Anyone)**
6. Klik **Terapkan (Deploy)**, lalu salin **URL Aplikasi Web**.
7. Buka Dashboard Banaho &rarr; klik menu **"Konfigurasi Google Sheet"** di sidebar &rarr; tempelkan URL tersebut pada kolom **URL Web App Google Apps Script**, lalu klik **Simpan & Sinkronkan**.

---

## 🔐 Sistem Autentifikasi Pengguna (Multi-User & Role)

Tersedia **3 peran akun bawaan (Default Login)**:

| Peran (Role) | Username | Password | Hak Akses & Fungsi |
| :--- | :--- | :--- | :--- |
| **👑 Pemilik / Administrator** | `admin` | `banaho123` | **Akses Penuh:** Monitoring omzet 2 tahun, konfigurasi sinkronisasi, data produksi, dan simulasi HPP. |
| **🍳 Tim Dapur & Produksi** | `produksi` | `dapur123` | **Fokus Dapur:** Membuka tab *Pencatatan Produksi*, kontrol stok bahan baku dapur, dan kalkulator rendemen. |
| **🛍️ Admin Penjualan & Kasir** | `penjualan` | `sales123` | **Fokus Reseller:** Membuka tab *Rekap Penjualan*, input transaksi offline/online, cetak nota pengiriman, dan WhatsApp. |

---

## 📈 Data Simulasi Penjualan 2 Tahun (2022 - 2023)

Dashboard dilengkapi dengan **dataset simulasi penjualan selama 2 tahun penuh (24 Bulan)** yang mempertahankan catatan pembukuan fisik asli Banaho (Januari 2022 & April 2023):

- **Total Transaksi:** 334 transaksi mitra
- **Total Omzet 2 Tahun:** **Rp 31.740.000**
  - **Tahun 2022:** Rp 16.140.000 (172 transaksi)
  - **Tahun 2023:** Rp 15.600.000 (162 transaksi)
- **Total Volume Distribusi:**
  - Kemasan Standing Pouch 80g: **4.045 Pcs**
  - Curah Kiloan Reseller: **273.0 Kg**

---

## 🚀 Cara Publish Gratis ke GitHub Pages

1. Buat repositori baru di [GitHub](https://github.com/new) (misal: `sim-banaho-magetan`). Pastikan diset **Public**.
2. Unggah file `index.html`, `sw.js`, dan `README.md` ke root repositori tersebut.
3. Di halaman repositori GitHub Anda:
   - Masuk ke tab **Settings** &rarr; menu **Pages** (di sisi kiri).
   - Pada bagian **Build and deployment** &rarr; **Branch**, pilih branch `main` dan folder `/(root)`.
   - Klik **Save**.
4. Website dashboard Anda langsung aktif di:
   `https://<username-anda>.github.io/sim-banaho-magetan/`
