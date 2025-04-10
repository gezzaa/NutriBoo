<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Status Gizi Bayi - NutriBoo</title>
  <style>
    body {
      margin: 0;
      font-family: 'Segoe UI', sans-serif;
      background: #fce4ec;
      color: #333;
    }

    header {
      background: linear-gradient(to right, #f06292, #64b5f6);
      color: white;
      padding: 20px;
      text-align: center;
    }

    .container {
      padding: 20px;
    }

    input, select, button {
      width: 100%;
      padding: 12px;
      margin-top: 10px;
      border: 1px solid #ccc;
      border-radius: 8px;
      font-size: 16px;
    }

    button {
      background-color: #f06292;
      color: white;
      border: none;
      cursor: pointer;
      transition: background 0.3s ease;
    }

    button:hover {
      background-color: #ec407a;
    }

    .result {
      margin-top: 20px;
      background-color: #ffffffdd;
      padding: 15px;
      border-radius: 10px;
    }

    h2 {
      color: #ec407a;
    }

    .status-baik {
      color: #2e7d32;
      font-weight: bold;
    }

    .status-kurang {
      color: #ef6c00;
      font-weight: bold;
    }

    .status-buruk {
      color: #c62828;
      font-weight: bold;
    }

    .tips {
      margin-top: 20px;
      background-color: #f8bbd0;
      padding: 15px;
      border-radius: 10px;
    }

    .tips ul {
      margin-top: 10px;
    }
  </style>
</head>
<body>

<header>
  <h1>NutriBoo</h1>
  <p>Cek Status Gizi & Dapatkan Rekomendasi Zat Gizi untuk Buah Hati</p>
</header>

<div class="container">
  <h2>Masukkan Data Bayi</h2>
  <label>Umur (bulan):</label>
  <input type="number" id="umur" placeholder="Contoh: 8">

  <label>Berat Badan (kg):</label>
  <input type="number" id="berat" step="0.1" placeholder="Contoh: 7.8">

  <label>Jenis Kelamin:</label>
  <select id="kelamin">
    <option value="laki">Laki-laki</option>
    <option value="perempuan">Perempuan</option>
  </select>

  <button onclick="cekStatusGizi()">Cek Status Gizi</button>

  <div class="result" id="hasil"></div>
  <div class="tips" id="rekomendasi"></div>
</div>

<script>
  function cekStatusGizi() {
    const umur = parseInt(document.getElementById('umur').value);
    const berat = parseFloat(document.getElementById('berat').value);
    const kelamin = document.getElementById('kelamin').value;

    if (isNaN(umur) || isNaN(berat)) {
      document.getElementById('hasil').innerHTML = '<p>Mohon masukkan semua data dengan benar.</p>';
      document.getElementById('rekomendasi').innerHTML = '';
      return;
    }

    const standarBBU = {
      laki: [3.3, 4.5, 5.6, 6.4, 7.0, 7.5, 7.9, 8.3, 8.6, 8.9, 9.2, 9.4, 9.6, 9.8, 10.0, 10.2, 10.3, 10.5, 10.7, 10.9, 11.0, 11.2, 11.3, 11.5],
      perempuan: [3.2, 4.2, 5.1, 5.8, 6.4, 6.9, 7.3, 7.6, 7.9, 8.2, 8.5, 8.7, 8.9, 9.1, 9.3, 9.5, 9.7, 9.9, 10.0, 10.2, 10.3, 10.5, 10.6, 10.8]
    };

    const standar = standarBBU[kelamin];
    const umurIndex = Math.min(Math.max(umur - 1, 0), 23);
    const bbStandar = standar[umurIndex];

    let status = '';
    let kelas = '';

    if (berat < bbStandar * 0.7) {
      status = 'Gizi Buruk';
      kelas = 'status-buruk';
    } else if (berat < bbStandar * 0.9) {
      status = 'Gizi Kurang';
      kelas = 'status-kurang';
    } else if (berat <= bbStandar * 1.2) {
      status = 'Gizi Baik';
      kelas = 'status-baik';
    } else {
      status = 'Berisiko Gizi Lebih';
      kelas = 'status-kurang';
    }

    document.getElementById('hasil').innerHTML = `
      <h2>Hasil:</h2>
      <p>Standar BB untuk umur ${umur} bulan: <strong>${bbStandar} kg</strong></p>
      <p>Status Gizi: <span class="${kelas}">${status}</span></p>
    `;

    // Rekomendasi ASI / MPASI
    let rekomendasi = '';

    if (umur < 6) {
      rekomendasi = `
        <h3>🍼 Rekomendasi Pemberian ASI</h3>
        <p>Bayi usia <strong>${umur} bulan</strong> sebaiknya diberikan <strong>ASI eksklusif</strong> tanpa tambahan makanan atau minuman lain.</p>
        <ul>
          <li>Berikan ASI sesuai permintaan bayi, siang dan malam.</li>
          <li>Jangan beri air putih, madu, pisang, atau bubur sebelum usia 6 bulan.</li>
        </ul>
      `;
    } else {
      rekomendasi = `
        <h3>🍲 Rekomendasi MPASI</h3>
        <p>Bayi usia <strong>${umur} bulan</strong> sudah bisa mulai diberikan <strong>MPASI</strong> yang mengandung zat gizi lengkap untuk buah hati dan aman.</p>
        <ul>
          <li>Puree atau bubur nasi + telur/ikan + sayur halus</li>
          <li>Buah lunak seperti pisang, alpukat, pepaya</li>
          <li>Perkenalkan tekstur secara bertahap (halus → lumat → cincang)</li>
          <li>Berikan 2–3 kali makan utama + 1–2 kali snack per hari</li>
        </ul>
      `;
    }

    document.getElementById('rekomendasi').innerHTML = rekomendasi;
  }
</script>

</body>
</html>
