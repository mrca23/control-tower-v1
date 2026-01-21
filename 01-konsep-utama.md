# 01 — Konsep Utama Control Tower v1

## 1. Tujuan Dasar Aplikasi
Control Tower v1 adalah aplikasi internal yang dibangun untuk mengontrol paket **incoming** di lingkungan J&T Express pada level **Drop Point (DP)** dan lintas DP.

Aplikasi ini bertujuan untuk:
- Mengontrol kepatuhan **SLA 3 hari** sejak paket scan sampai di DP
- Mengurangi risiko **claim dan denda**
- Menyediakan visibilitas kendala dan penanganan paket
- Memastikan setiap paket yang terlambat memiliki **narasi penanganan yang jelas**

Control Tower v1 **bukan pengganti JMS** dan **tidak menjalankan proses operasional** (delivery atau retur).  
Aplikasi ini murni berfungsi sebagai **lapisan kontrol dan monitoring**.

---

## 2. Struktur Organisasi & Peran
Struktur peran yang terlibat dalam Control Tower v1:

- **Sprinter**
- **Admin**
- **Kordi**
- **SPV**

Kamu dan rekan kamu berperan sebagai **super user**, namun secara struktur organisasi, level tertinggi tetap **SPV**.

---

## 3. Definisi Operasional Dasar

### 3.1 Incoming
- Incoming dihitung **sejak paket scan sampai di Drop Point (DP)**.
- Titik scan ini menjadi **hari pertama (hari ke-1)** perhitungan SLA.

### 3.2 SLA 3 Hari
- Paket hanya boleh ditahan **maksimal 3 hari** sejak scan sampai di DP.
- **Hari ke-4** sudah masuk kategori berisiko tinggi (late).

### 3.3 Status Paket
- **Aktif**
  - Paket sudah scan sampai di DP
  - Belum scan TTD
  - Belum scan sampai station lain (retur)

- **Selesai**
  - Scan TTD (delivery sukses), atau
  - Scan sampai station lain (retur)

Tidak ada status selesai lain di Control Tower v1.

---

## 4. POD (Proof of Delivery – versi Control Tower)

### 4.1 Definisi POD
POD dalam konteks Control Tower v1 adalah **bukti komunikasi ke customer**, bukan TTD fisik.

### 4.2 Media POD
- Media utama: **WhatsApp**
- Media fallback: **SMS** (digunakan jika WA terkena spam/pembatasan)

Sprinter akan **mengunggah bukti POD melalui halaman web sprinter**.

Aplikasi Control Tower v1 **tidak mengirim pesan**, hanya:
- menerima upload bukti
- mencatat status POD

### 4.3 Syarat Minimal Bukti POD
Setiap bukti POD wajib memiliki:
- Nomor tujuan
- Isi pesan
- **Timestamp**
- **Lokasi**

### 4.4 Aturan 3 Hari Berbeda
- POD wajib tersedia pada **3 hari yang berbeda**
- Jika tidak lengkap → **POD dianggap tidak valid**
- Namun:
  - **Validasi POD tidak dilakukan di v1**
  - Di v1, POD hanya dicatat sebagai:
    - **ADA**
    - **TIDAK ADA**

Validasi detail masuk fitur lanjutan.

---

## 5. Feedback & Penanganan Paket

### 5.1 Prinsip Feedback
Control Tower v1 menempatkan feedback sebagai **inti kontrol**, bukan status.

Prinsip utama:
> **Tidak ada AWB tanpa cerita**

### 5.2 Sumber Feedback
Feedback dapat diberikan oleh:
- **Sprinter** (kendala lapangan & penanganan)
- **Admin**
- **Kordi**
- **SPV**

Di Control Tower v1:
- Feedback **belum menggunakan label/kategori**
- Semua feedback dicatat sebagai narasi berbasis waktu

---

### 5.3 Feedback Harus Selalu Ter-update
Feedback harus mencerminkan kondisi lapangan terkini, misalnya:
- Paket hilang
- Jadwal ulang dengan customer
- Paket sukses tapi lupa scan TTD
- Paket rusak dan perlu investigasi
- dll

Jika kondisi berubah, feedback **wajib diperbarui**.

---

### 5.4 Aturan Wajib untuk Paket Late
Jika paket sudah **lebih dari 3 hari (late)**:
- **WAJIB ada minimal 1 feedback per hari**
- Berlaku sampai paket berstatus **selesai**

Contoh:
AWB sudah 5 hari di DP  
→ harus ada feedback hari ke-1, ke-2, ke-3, ke-4, dan ke-5.

---

## 6. Model Admin di Drop Point

### 6.1 Admin Pelayanan
- Bertugas melayani customer di DP
- Dapat mengambil **foto customer** sebagai kontrol internal
- Foto ini:
  - untuk pengingat internal
  - **bukan POD resmi**

---

### 6.2 Admin Retur
- Fokus pada:
  - POD komunikasi
  - Proses retur
- Berkaitan langsung dengan paket late

---

### 6.3 Admin Processing / Gudang
- Fokus pada kondisi fisik paket
- Dapat mengambil foto:
  - packing rusak
  - paket bocor
  - kondisi mencurigakan
- Digunakan untuk investigasi dan catatan internal

---

## 7. Monitoring & Report

### 7.1 Kordi
- Memonitor **1 DP**
- Melihat:
  - SLA paket
  - Kendala
  - POD ada / tidak ada
- Report difokuskan pada kontrol harian

---

### 7.2 SPV
- Memonitor **lebih dari 1 DP**
- Report berbasis **per Drop Point**
- Dapat drill-down ke detail saat klik total di tabel

---

## 8. Output Utama Control Tower v1

### 8.1 SLA (Terpisah dari POD)
- SLA dinilai dari **paket terlama (aging tertinggi)**
- Digunakan untuk:
  - ranking sprinter (di sisi kordi)
  - ranking DP (di sisi SPV)

---

### 8.2 POD (Terpisah dari SLA)
- POD diukur dari **persentase ADA POD**
- Diurutkan dari terbaik sampai terburuk

---

### 8.3 Report Per Tanggal Scan Sampai
- Sistem wajib menampilkan **semua tanggal scan sampai**
- Untuk setiap tanggal ditampilkan:
  - jumlah AWB yang masih ada di DP
- “Masih ada” berarti:
  - belum TTD
  - belum scan sampai station lain (retur)

---

## 9. Batasan Control Tower v1 (Fitur Lanjutan)
Fitur berikut **tidak dikerjakan di v1**:
- Tracking outgoing
- Validasi POD
- Label/kategori feedback
- Logika scan JMS lanjutan

Semua batasan ini adalah keputusan sadar untuk menjaga v1 tetap fokus dan bisa dijalankan di lapangan.
