<!-- Ganti URL di bawah dengan URL Aplikasi Web Baru hasil Langkah 2 -->
<form id="radiologiForm" action="[URL_APL_WEB_BARU_ANDA](https://script.google.com/macros/s/AKfycbyS5HwEZ153pPM258J8bYMjGJTe2zeBN_NOW9hkvl6EmNY87JuappUzRa8RCz92y9KV-g/exec )" method="POST">
  
  <label>Nomor ID</label>
  <input type="text" name="nomorID" required>

  <label>Nama Pasien</label>
  <input type="text" name="namaPasien" required>

  <label>Tanggal Lahir</label>
  <input type="date" name="tanggalLahir" required>

  <label>Jenis Kelamin</label>
  <select name="jenisKelamin" required>
    <option value="">Pilih</option>
    <option value="Laki-laki">Laki-laki</option>
    <option value="Perempuan">Perempuan</option>
  </select>

  <label>Nomor WhatsApp</label>
  <input type="tel" name="whatsapp" required>

  <label>Dokter Pengirim</label>
  <input type="text" name="dokter">

  <label>Jenis Pemeriksaan</label>
  <select name="jenisPemeriksaan" required>
    <option value="">Pilih Pemeriksaan</option>
    <option value="Rontgen">Rontgen</option>
    <option value="USG">USG</option>
  </select>

  <label>Keterangan Klinis</label>
  <textarea name="keterangan"></textarea>

  <button type="submit">Simpan Data Pasien</button>
</form>
