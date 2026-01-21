# Control Tower v1

## Latar Belakang
Control Tower v1 adalah aplikasi internal yang dibangun untuk membantu pengawasan operasional paket **incoming** di lingkungan J&T Express, khususnya pada level **Drop Point (DP)** dan lintas DP.

Aplikasi ini **bukan pengganti JMS**, dan **tidak menjalankan eksekusi operasional**, melainkan berfungsi sebagai **lapisan kontrol, monitoring, dan akuntabilitas** untuk mengurangi risiko **claim dan denda** akibat pelanggaran SLA.

---

## Masalah yang Ingin Diselesaikan
Di lapangan, paket yang sudah scan sampai di DP sering:
- tertahan lebih dari 3 hari,
- tidak jelas penanganannya,
- tidak terdokumentasi kendala hariannya,
- tidak punya bukti komunikasi yang rapi,
- dan sulit dimonitor secara cepat oleh kordi maupun SPV.

Sistem utama (JMS) tidak menyediakan visibilitas narasi harian dan tidak semua data yang dibutuhkan bisa dikontrol atau ditarik.

---

## Tujuan Control Tower v1
- Mengontrol kepatuhan **SLA 3 hari** sejak paket scan sampai di DP
- Mengidentifikasi paket yang masuk kategori **late**
- Memastikan setiap paket late memiliki **penanganan harian**
- Memberikan visibilitas kendala & status ke semua level (sprinter, admin, kordi, SPV)
- Menjadi alat kontrol internal untuk mitigasi claim & denda

---

## Prinsip Dasar
1. Control Tower v1 adalah **alat kontrol**, bukan alat eksekusi
2. **Tidak ada AWB tanpa cerita**
3. Feedback lebih penting dari sekadar status
4. Semakin tinggi role, semakin ringkas data, tetapi tetap bisa ditelusuri ke AWB

---

## Batasan v1
Fitur berikut **sengaja tidak dikerjakan di v1**:
- Tracking outgoing
- Validasi POD
- Kategori/label feedback
- Logika scan JMS lanjutan

Dokumen detail konsep terdapat pada file lanjutan di repository ini.
