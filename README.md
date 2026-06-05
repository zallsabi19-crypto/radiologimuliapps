# 🏥 Radiologi MuliApps

Radiologi MuliApps adalah sistem informasi manajemen data pasien radiologi berbasis web yang terintegrasi secara otomatis dengan **Google Sheets** sebagai basis data dan **Google Docs** sebagai mesin pembuat laporan otomatis (*automated reporting engine*). 

Sistem ini dirancang untuk memangkas waktu administrasi klinis, meminimalkan kesalahan input manual, serta mempercepat distribusi hasil pemeriksaan radiologi dari admin ke dokter spesialis.

---

## 🚀 Fitur Utama

*   **Formulir Input Responsif**: Antarmuka web yang bersih, valid, dan mudah digunakan oleh staf admin/radiografer.
*   **Sinkronisasi Real-Time**: Data yang diinput langsung mengalir ke Google Sheets tanpa penundaan (*zero-lag*).
*   **Pencetakan Laporan Otomatis**: Memanfaatkan Google Apps Script untuk menduplikasi template dokumen laporan radiologi secara instan begitu data pasien disimpan.
*   **Bypass Penamaan Sheet**: Menggunakan sistem indeks berbasis array (`.getSheets()[0]`) sehingga sistem tetap berjalan mulus meskipun ada perubahan nama tab pada spreadsheet.
*   **Penanganan Error Terpusat**: Dilengkapi dengan blok `try-catch` pada backend untuk memastikan log error tercatat dengan baik jika terjadi kegagalan sistem.

---

## 🛠️ Arsitektur & Teknologi

Sistem ini dibangun menggunakan ekosistem teknologi yang ringan namun bertenaga:

*   **Frontend**: HTML5, CSS3 (Google Fonts Integration), dan JavaScript modern.
*   **Backend / API**: Google Apps Script (Engine: V8 Runtime).
*   **Database**: Google Sheets (Sebagai repositori data pasien).
*   **Document Generator**: Google Docs API via Apps Script Core Service.
*   **Hosting**: GitHub Pages (Untuk deployment antarmuka web).

---

## ⚙️ Panduan Instalasi & Konfigurasi

### 1. Struktur Backend (Google Apps Script)
Tempelkan kode berikut pada file `Code.gs` di proyek Apps Script yang terhubung dengan Google Sheets Anda:

```javascript
function doPost(e) {
  try {
    var sheet = SpreadsheetApp.getActiveSpreadsheet().getSheets()[0];
    var timestamp = new Date();
    
    var nomorID = e.parameter.nomorID;
    var namaPasien = e.parameter.namaPasien;
    var tanggalLahir = e.parameter.tanggalLahir;
    var jenisKelamin = e.parameter.jenisKelamin;
    var whatsapp = e.parameter.whatsapp;
    var dokter = e.parameter.dokter;
    var jenisPemeriksaan = e.parameter.jenisPemeriksaan;
    var keterangan = e.parameter.keterangan;
    
    sheet.appendRow([timestamp, nomorID, namaPasien, tanggalLahir, jenisKelamin, whatsapp, dokter, jenisPemeriksaan, keterangan]);
    
    buatLaporanOtomatis(nomorID, namaPasien, tanggalLahir, jenisKelamin, whatsapp, dokter, jenisPemeriksaan, keterangan, timestamp);
    
    return ContentService.createTextOutput(JSON.stringify({"result":"success", "message": "Data & Laporan Berhasil Dibuat!"}))
                         .setMimeType(ContentService.MimeType.JSON);
  } catch(error) {
    return ContentService.createTextOutput(JSON.stringify({"result":"error", "error": error.toString()}))
                         .setMimeType(ContentService.MimeType.JSON);
  }
}

function buatLaporanOtomatis(nomorID, namaPasien, tanggalLahir, jenisKelamin, whatsapp, dokter, jenisPemeriksaan, keterangan, timestamp) {
  var TEMPLATE_ID = "1Vt5By9KMQ8JajDmb_ozOywGKj0sfEHe_My29fisL-0c"; 
  var tglPeriksaFormatted = Utilities.formatDate(timestamp, "GMT+7", "dd/MM/yyyy");
  
  var namaFileBaru = "Laporan Radiologi - " + namaPasien + " (" + nomorID + ")";
  var copyFile = DriveApp.getFileById(TEMPLATE_ID).makeCopy(namaFileBaru);
  var copyDoc = DocumentApp.openById(copyFile.getId());
  var body = copyDoc.getBody();
  
  body.replaceText("{{tanggalPeriksa}}", tglPeriksaFormatted);
  body.replaceText("{{nomorID}}", nomorID || "-");
  body.replaceText("{{namaPasien}}", namaPasien || "-");
  body.replaceText("{{tanggalLahir}}", tanggalL
