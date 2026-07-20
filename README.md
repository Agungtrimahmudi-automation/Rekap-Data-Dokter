# Sistem Rekap Data Kunjungan Dokter Otomatis

> Otomatisasi rekap data dari 3 sumber berbeda menjadi laporan lengkap dalam 1 menit.

---

## 📌 Ringkasan

Sistem ini mengotomatisasi proses rekap data kunjungan dokter di rumah sakit. Data dari 3 sumber (Rawat Inap, Poli, ICU) dengan format berbeda digabung, diproses, dan disajikan sebagai laporan harian per dokter.

| Metrik | Sebelum | Sesudah | Penghematan |
| :--- | :--- | :--- | :--- |
| **Waktu Pengerjaan** | 2 jam | 1 menit | **95%** |
| **Kesalahan Hitung** | Sering terjadi | Hampir nol | - |
| **Status** | Manual | Otomatis | - |

**Status:** Dipakai nyata oleh staf tu rumah sakit busel

---

## 🧠 Problem

### Masalah yang Dihadapi Staf Rumah Sakit

| Masalah | Detail |
| :--- | :--- |
| **3 Sumber Data Berbeda** | Data berasal dari 3 bagian: Rawat Inap, Poli, dan ICU. Formatnya berbeda-beda. |
| **Format Tanggal Beda** | Ada yang DD/MM/YYYY, ada yang MM/DD/YYYY, ada yang pakai strip, ada yang pakai slash. Excel bingung. |
| **Rentang Tanggal** | Pasien rawat inap punya tanggal masuk dan keluar (misal 2-5 Juni). Dalam Excel cuma 1 baris, tapi harus dipecah per hari untuk laporan. |
| **VLOOKUP Tidak Cukup** | Karena format tanggal beda dan ada rentang, VLOOKUP tidak bisa langsung. Staf harus: <br> 1. Hitung manual berapa hari setiap pasien <br> 2. Pecah rentang tanggal jadi per hari <br> 3. Baru bisa pakai VLOOKUP |
| **Grouping per Dokter** | Setelah data jadi per hari, masih harus dikelompokkan per dokter dan dihitung jumlah pasien per hari. |
| **Laporan 30 Hari** | Harus menghasilkan laporan untuk 30 hari, dengan data per dokter per hari. |
| **Waktu & Risiko Error** | Semua manual → butuh **2 jam** dan **rawan salah hitung**. |

### Dampak

- Staf kerja lembur setiap bulan
- Laporan sering direvisi karena kesalahan hitung
- Proses membosankan dan repetitif
- Waktu yang seharusnya untuk hal lebih penting terbuang

---

## 💡 Solution

### Pendekatan

Saya membangun **sistem otomatisasi end-to-end** yang menangani seluruh proses dari hulu ke hilir.

### Alur Kerja

Google Form (Upload 3 File Excel) -> n8n Webhook (Terima Data) -> Baca 3 File Excel (Rawat Inap, Poli, ICU)

Format Data:

Normalisasi format tanggal -> Pecah rentang tanggal per hari -> Gabungkan Semua Data (Merge) -> Kelompokkan per Dokter per Hari -> Hitung Jumlah Pasien per Hari ->
Simpan ke Google Sheets: Sheet Dokter A, Sheet Dokter B, Sheet Dokter C, Sheet Dokter D -> Kirim Email Notifikasi ke Staf


### Teknologi yang Digunakan

| Teknologi | Fungsi |
| :--- | :--- |
| **n8n** | Workflow orchestration |
| **Google Sheets API** | Baca data sumber & tulis hasil |
| **Google Drive API** | Simpan file output |
| **Google Form** | Input upload file dari staf |
| **Google Apps Script** | Kirim data form ke webhook |
| **JavaScript** | Logika pemrosesan data di n8n |
| **Gmail API** | Kirim notifikasi email |

### Fitur Utama

- ✅ Upload 3 file sekaligus melalui Google Form
- ✅ Normalisasi format tanggal otomatis
- ✅ Pecah rentang tanggal per hari secara otomatis
- ✅ Gabung 3 sumber data tanpa VLOOKUP
- ✅ Kelompokkan per dokter dan hitung jumlah pasien
- ✅ Output 4 sheet terpisah di 1 file Google Sheets
- ✅ Email notifikasi otomatis ke staf
- ✅ Error handling jika ada masalah data

---

## 📊 Result

### Dampak Nyata

| Metrik | Sebelum | Sesudah | Penghematan |
| :--- | :--- | :--- | :--- |
| **Waktu Pengerjaan** | 2 jam | 1 menit | **95%** |
| **Kesalahan Hitung** | Sering terjadi | Hampir nol | - |
| **Lembur Staf** | Setiap bulan | Tidak ada | - |
| **Status** | Manual | Otomatis | - |

### Output

**1 file Google Sheets dengan 4 sheet:**

| Sheet | Isi |
| :--- | :--- |
| **Rizal** | Data harian dr. Rizal Fahly, Sp.PD |
| **Sarning** | Data harian dr. Wa Ode Sarning S, Sp.PD |
| **Putri** | Data harian dr. Putri Hidayasyah Purnama Lestari Sp.PK |
| **Yanti** | Data harian dr. Mulyanti Sulastri Sp.GK |

**Struktur setiap sheet:**

| Kolom | Keterangan |
| :--- | :--- |
| No | Nomor urut per dokter |
| NIK Dokter | NIK dokter |
| Nama Dokter | Nama lengkap dokter |
| Tanggal Pelayanan | Format DD/MM/YYYY |
| Jumlah Pasien | Total pasien pada tanggal itu |
| Jumlah Bacaan Penunjang | Total bacaan penunjang (sama dengan jumlah pasien) |

### Nilai Tambah

- ✅ **95% lebih cepat** — dari 2 jam jadi 1 menit
- ✅ **0 kesalahan hitung** — semua otomatis
- ✅ **Fokus ke hal penting** — staf tidak perlu lembur
- ✅ **Sistem berjalan konsisten** — setiap bulan tinggal upload

---

## 🛠️ Cara Kerja

### Bagi Staf (Pengguna)

1. Buka Google Form
2. Upload 3 file Excel (Rawat Inap, Poli, ICU)
3. Isi bulan target (contoh: `06/2026`)
4. Klik Submit
5. Tunggu email notifikasi (1-2 menit)
6. Buka link Google Sheets → laporan sudah siap

### Bagi Developer

1. Clone repository ini
2. Import workflow ke n8n
3. Setup Google Sheets credential
4. Setup Gmail credential
5. Aktifkan workflow
6. Workflow siap dipakai

---

## 📸 Screenshot

| Google Form | Workflow n8n | Hasil Output |
| :--- | :--- | :--- |
| ![Form](assets/form.png) | ![Workflow](assets/workflow.png) | ![Output](assets/output.png) |

---

## 🔗 Demo Video

[Link video demo]

---

## 📌 Teknologi

![n8n](https://img.shields.io/badge/n8n-0A0A0A?style=for-the-badge&logo=n8n&logoColor=white)
![Google Sheets](https://img.shields.io/badge/Google%20Sheets-34A853?style=for-the-badge&logo=googlesheets&logoColor=white)
![Google Drive](https://img.shields.io/badge/Google%20Drive-4285F4?style=for-the-badge&logo=googledrive&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black)

---

## 👤 Author

**Agung Tri Mahmudi**

- Email: agungtrimahmudi.it@gmail.com
- GitHub: [github.com/agungtm](https://github.com/agungtm)
- LinkedIn: [linkedin.com/in/agung-tri-mahmudi-909a663b4](https://linkedin.com/in/agung-tri-mahmudi-909a663b4)

---

## 📄 Lisensi

Proyek ini dibuat untuk portofolio. Bebas digunakan dengan mencantumkan kredit.
