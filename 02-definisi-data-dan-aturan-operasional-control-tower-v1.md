# 02 — Definisi Data & Aturan Operasional (Control Tower v1 — DP Labuha)

## 1. Ruang Lingkup
Dokumen ini mendefinisikan aturan data dan operasional Control Tower v1 berbasis sampel aktif pada Drop Point **LABUHA**.

Control Tower v1 adalah **sistem kontrol & monitoring**, bukan sistem eksekusi operasional.

Jika paket **keluar dari DP Labuha**, maka paket tersebut **dikeluarkan dari Control Tower** (perspektif kontrol DP Labuha).

---

## 2. Sumber Data (Input Resmi)

### 2.1 Input 1 — Monitor Scan Sampai (JMS)
Fungsi utama:
> Baseline semua AWB incoming yang scan sampai di DP

Kolom yang digunakan (nama kolom harus sama persis; urutan bebas):
- No. Waybill
- Sumber Order
- Drop Point
- Waktu Sampai
- Lokasi Sebelumnya
- Discan oleh

Catatan:
- Semua AWB yang masuk kontrol Control Tower DP Labuha berasal dari input ini.
- Waktu Sampai menjadi dasar penentuan **hari ke-1** (berbasis tanggal kalender).

---

### 2.2 Input 2 — Status Terupdate (JMS)
Fungsi utama:
- Menentukan paket keluar dari kontrol DP Labuha (Exit Rule)
- Menentukan selesai/sukses (Completion Rule)
- Menentukan misroute
- Memuat informasi status scan ter-update

Kolom yang digunakan (nama kolom harus sama persis; urutan bebas):
- No. Waybill
- DP NLC
- Biaya COD
- total DFOD
- Station Scan
- Jenis Scan
- Waktu Scan
- Alasan Masalah/Tinggal Gudang

---

### 2.3 Input 3 — Pencarian Waybill Incoming (JMS)
Fungsi utama:
- Informasi detail per AWB terkait penerima
- Informasi sprinter delivery
- Informasi pembayaran COD

Kolom yang digunakan (nama kolom harus sama persis; urutan bebas):
- No. Waybill
- Penerima
- Alamat Penerima
- HP Penerima
- Kode Sprinter Delivery
- Sprinter Delivery
- Pembayaran COD

---

## 3. Aturan Status Paket (Perspektif Control Tower DP Labuha)

### 3.1 Paket Aktif
Paket dianggap **AKTIF** selama:
- AWB tercatat di Input 1, dan
- Belum memenuhi kriteria “keluar dari Control Tower” (lihat 3.2).

### 3.2 Paket Keluar dari Control Tower (DP Labuha)
AWB **dikeluarkan** dari kontrol Control Tower DP Labuha jika memenuhi salah satu kondisi berikut (berdasarkan Input 2):

#### A) Keluar DP (Exit Rule)
- `Station Scan` ≠ `LABUHA`

#### B) Selesai / Sukses (Completion Rule)
- `Jenis Scan` = `Scan TTD`
- `Jenis Scan` = `Scan TTD Retur`

Catatan:
- Setelah memenuhi Exit Rule atau Completion Rule, paket tidak lagi dimonitor di Control Tower DP Labuha.

---

## 4. Aturan Misroute
Paket dikategorikan **MISROUTE** jika (berdasarkan Input 2):
- `DP NLC` ≠ `LABUHA`

---

## 5. Aturan Hitung Hari & SLA (Hari Kalender)

### 5.1 Dasar Hitung Hari
- Perhitungan hari menggunakan **hari kalender**.
- Pergantian hari terjadi saat **tanggal berubah**, tidak bergantung jam.

### 5.2 Definisi Hari
- Hari ke-1 = tanggal pada `Waktu Sampai` (Input 1)
- Hari ke-2 = tanggal berikutnya
- dst

Contoh:
- Paket masuk tanggal 1 jam 15:00 → tetap hari ke-1
- Tanggal 2 jam 07:00 → dianggap hari ke-2
- Tanggal 4 → hari ke-4 (late)

### 5.3 Definisi Late
- Paket dianggap **LATE** pada **hari ke-4 kalender** sejak tanggal `Waktu Sampai`.

---

## 6. Aturan Feedback & Penanganan

### 6.1 Prinsip
Tidak ada AWB tanpa cerita. Feedback adalah narasi penanganan, bukan sekadar status.

### 6.2 Kewajiban Feedback
- Default: feedback wajib mulai **hari ke-4 (late)**.
- Pengecualian: feedback boleh muncul sebelum hari ke-4 jika ada masalah (contoh: packing rusak, merembes, dll).

### 6.3 Aturan Harian Feedback untuk Paket Late
- Untuk paket late: minimal **1 feedback per hari kalender** sampai paket keluar dari Control Tower DP Labuha.

---

## 7. Ringkasan Peran Input
- Input 1 = mengunci daftar AWB incoming (baseline)
- Input 2 = menentukan status scan ter-update + exit/selesai + misroute
- Input 3 = melengkapi detail penerima + sprinter delivery + pembayaran COD

---

## 8. Batasan v1
Mengikuti batasan yang sudah dikunci di dokumen konsep utama.
