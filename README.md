# Arus_kas.indeks
arus kas PT GISAI SOLUSI MANDIRI
echo "# Arus_kas.indeks" >> README.md
git init
git add README.md
git commit -m "first commit"
git branch -M main
git remote add origin https://github.com/idoya078-debug/Arus_kas.indeks.git
git push -u origin main<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Arus Kas Perusahaan</title>
    <style>
        * { font-family: Arial, sans-serif; margin: 0; padding: 0; box-sizing: border-box; }
        body { background: #f5f5f5; padding: 15px; max-width: 980px; margin: auto; }
        h1 { text-align: center; color: #2c3e50; margin-bottom: 20px; font-size: 20px; }
        .card { background: white; padding: 15px; border-radius: 8px; box-shadow: 0 2px 6px rgba(0,0,0,0.1); margin-bottom: 15px; }
        h2 { color: #34495e; margin-bottom: 12px; font-size: 16px; border-bottom: 1px solid #eee; padding-bottom: 5px; }
        .form-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(150px, 1fr)); gap: 10px; margin-bottom: 12px; }
        input, select, button { padding: 10px; border: 1px solid #ddd; border-radius: 4px; font-size: 14px; width: 100%; }
        button { background: #27ae60; color: white; border: none; cursor: pointer; font-weight: bold; }
        .btn-merah { background: #e74c3c; }
        .btn-biru { background: #3498db; }
        .btn-abu { background: #95a5a6; }
        .saldo { font-size: 20px; font-weight: bold; color: #2980b9; text-align: center; padding: 15px; background: #ecf0f1; border-radius: 6px; }
        table { width: 100%; border-collapse: collapse; margin-top: 10px; font-size: 13px; }
        th, td { padding: 10px; text-align: left; border-bottom: 1px solid #eee; }
        th { background: #f8f9fa; }
        .masuk { color: #27ae60; font-weight: bold; }
        .keluar { color: #e74c3c; font-weight: bold; }
        .laporan { display: grid; grid-template-columns: repeat(2, 1fr); gap: 10px; margin-top: 12px; }
        .lap-box { padding: 12px; border-radius: 6px; color: white; font-weight: bold; text-align: center; }
        .lap-masuk { background: #27ae60; }
        .lap-keluar { background: #e74c3c; }
        .lap-selisih { background: #f39c12; }
        .lap-saldo { background: #9b59b6; }
        .filter-area, .cari-area, .rekap-bulan { display: flex; flex-wrap: wrap; gap: 8px; margin-bottom: 12px; align-items: center; }
        .login-box { max-width: 350px; margin: 40px auto; text-align: center; }
        .tersembunyi { display: none; }
        .info-pengguna { text-align: right; margin-bottom: 10px; font-size: 14px; }
    </style>
</head>
<body>

<div id="halamanLogin" class="card login-box">
    <h2>🔐 Masuk Sistem</h2>
    <input type="text" id="username" placeholder="Nama Pengguna">
    <input type="password" id="password" placeholder="Kata Sandi">
    <select id="peran">
        <option value="staf">Staf Keuangan</option>
        <option value="admin">Admin</option>
    </select>
    <button onclick="login()">Masuk</button>
    <p style="margin-top:10px; font-size:12px; color:#666;">
        Contoh: Admin = admin / 1234<br>Staf = staf / 1234
    </p>
</div>

<div id="halamanUtama" class="tersembunyi">
    <h1>📊 APLIKASI ARUS KAS</h1>
    <div class="info-pengguna">
        Pengguna: <span id="namaPengguna"></span>
        <button class="btn-merah" onclick="keluar()" style="width:auto; padding:5px 10px;">Keluar</button>
    </div>

    <div class="card">
        <h2>Saldo Kas Saat Ini</h2>
        <div class="form-grid" style="grid-template-columns: 2fr 1fr;">
            <input type="number" id="saldoAwalInput" placeholder="Saldo Awal (Rp)">
            <button class="btn-biru" onclick="simpanSaldoAwal()">Atur</button>
        </div>
        <div class="saldo">Saldo: <span id="saldoAkhir">Rp 0</span></div>
    </div>

    <div class="card">
        <h2>Tambah Transaksi</h2>
        <div class="form-grid">
            <input type="date" id="tanggal">
            <select id="jenis">
                <option value="">Pilih Jenis</option>
                <option value="Masuk">Kas Masuk</option>
                <option value="Keluar">Kas Keluar</option>
            </select>
            <select id="kategori">
                <option value="">Pilih Kategori</option>
                <optgroup label="Masuk">
                    <option value="Penjualan">Penjualan</option>
                    <option value="Piutang">Penerimaan Piutang</option>
                    <option value="Pendapatan Lain">Pendapatan Lain</option>
                </optgroup>
                <optgroup label="Keluar">
                    <option value="Pembelian">Pembelian Barang</option>
                    <option value="Gaji">Gaji & Upah</option>
                    <option value="Operasional">Biaya Operasional</option>
                    <option value="Sewa">Biaya Sewa</option>
                    <option value="Utang">Bayar Utang</option>
                    <option value="Lainnya">Biaya Lain</option>
                </optgroup>
            </select>
            <input type="number" id="jumlah" placeholder="Jumlah (Rp)">
            <input type="text" id="keterangan" placeholder="Keterangan">
            <button onclick="tambahTransaksi()">Simpan</button>
        </div>
    </div>

    <!-- ✅ BAGIAN REKAP PER BULAN -->
    <div class="card">
        <h2>📅 Rekap Per Bulan</h2>
        <div class="rekap-bulan">
            <select id="pilihBulan">
                <option value="01">Januari</option>
                <option value="02">Februari</option>
                <option value="03">Maret</option>
                <option value="04">April</option>
                <option value="05">Mei</option>
                <option value="06">Juni</option>
                <option value="07">Juli</option>
                <option value="08">Agustus</option>
                <option value="09">September</option>
                <option value="10">Oktober</option>
                <option value="11">November</option>
                <option value="12">Desember</option>
            </select>
            <input type="number" id="pilihTahun" min="2020" max="2035" value="2026">
            <button class="btn-biru" onclick="tampilkanRekapBulan()">Lihat Rekap</button>
            <button class="btn-abu" onclick="resetFilter()">Tampilkan Semua</button>
            <button class="btn-merah" onclick="eksporKeCSV()">Ekspor Rekap</button>
        </div>
    </div>

    <div class="card">
        <h2>Daftar Transaksi</h2>
        <div class="cari-area">
            <input type="text" id="kataKunci" placeholder="Cari...">
            <button class="btn-biru" onclick="cariTransaksi()">Cari</button>
        </div>
        <table>
            <thead>
                <tr>
                    <th>Tanggal</th>
                    <th>Jenis</th>
                    <th>Kategori</th>
                    <th>Jumlah</th>
                    <th>Keterangan</th>
                </tr>
            </thead>
            <tbody id="tabelTransaksi"></tbody>
        </table>
    </div>

    <div class="card">
        <h2>📊 Ringkasan Periode</h2>
        <div class="laporan">
            <div class="lap-box lap-masuk">
                <p>Total Masuk</p>
                <p id="totalMasuk">Rp 0</p>
            </div>
            <div class="lap-box lap-keluar">
                <p>Total Keluar</p>
                <p id="totalKeluar">Rp 0</p>
            </div>
            <div class="lap-box lap-selisih">
                <p>Selisih</p>
                <p id="selisih">Rp 0</p>
            </div>
            <div class="lap-box lap-saldo">
                <p>Saldo Akhir</p>
                <p id="saldoLaporan">Rp 0</p>
            </div>
        </div>
    </div>
</div>

<script>
const daftarPengguna = [
    { username: "admin", sandi: "1234", peran: "admin", nama: "Admin" },
    { username: "staf", sandi: "1234", peran: "staf", nama: "Staf" }
];
let penggunaAktif = null;
let transaksi = JSON.parse(localStorage.getItem("dataKas")) || [];
let saldoAwal = Number(localStorage.getItem("saldoAwal")) || 0;
let dataTampil = [...transaksi];

function formatRupiah(angka) {
    return "Rp " + angka.toLocaleString("id-ID");
}

function login() {
    const user = document.getElementById("username").value.trim();
    const pass = document.getElementById("password").value.trim();
    const peran = document.getElementById("peran").value;
    const cocok = daftarPengguna.find(p => p.username === user && p.sandi === pass && p.peran === peran);
    if (cocok) {
        penggunaAktif = cocok;
        document.getElementById("namaPengguna").textContent = cocok.nama;
        document.getElementById("halamanLogin").classList.add("tersembunyi");
        document.getElementById("halamanUtama").classList.remove("tersembunyi");
        document.getElementById("saldoAwalInput").value = saldoAwal;
        tampilkanData();
    } else alert("Data masuk salah!");
}

function keluar() {
    penggunaAktif = null;
    document.getElementById("halamanUtama").classList.add("tersembunyi");
    document.getElementById("halamanLogin").classList.remove("tersembunyi");
}

function simpanSaldoAwal() {
    if (penggunaAktif.peran !== "admin") return alert("Hanya admin yang boleh mengubah saldo awal!");
    const nilai = Number(document.getElementById("saldoAwalInput").value);
    if (nilai >= 0) {
        saldoAwal = nilai;
        localStorage.setItem("saldoAwal", saldoAwal);
        tampilkanData();
        alert("Saldo awal tersimpan!");
    }
}

function tampilkanData(data = dataTampil) {
    const tbody = document.getElementById("tabelTransaksi");
    tbody.innerHTML = "";
    let saldo = saldoAwal, masuk = 0, keluar = 0;
    data.sort((a,b) => new Date(a.tanggal) - new Date(b.tanggal));
    data.forEach(item => {
        if (item.jenis === "Masuk") { saldo += item.jumlah; masuk += item.jumlah; }
        else { saldo -= item.jumlah; keluar += item.jumlah; }
        const baris = document.createElement("tr");
        baris.innerHTML = `<td>${item.tanggal}</td><td class="${item.jenis === "Masuk" ? "masuk" : "keluar"}">${item.jenis}</td><td>${item.kategori}</td><td class="${item.jenis === "Masuk" ? "masuk" : "keluar"}">${formatRupiah(item.jumlah)}</td><td>${item.keterangan || "-"}</td>`;
        tbody.appendChild(baris);
    });
    document.getElementById("saldoAkhir").textContent = formatRupiah(saldo);
    document.getElementById("totalMasuk").textContent = formatRupiah(masuk);
    document.getElementById("totalKeluar").textContent = formatRupiah(keluar);
    document.getElementById("selisih").textContent = formatRupiah(masuk - keluar);
    document.getElementById("saldoLaporan").textContent = formatRupiah(saldo);
}

// ✅ Fungsi Rekap Per Bulan
function tampilkanRekapBulan() {
    const bulan = document.getElementById("pilihBulan").value;
    const tahun = document.getElementById("pilihTahun").value;
    dataTampil = transaksi.filter(item => item.tanggal.startsWith(`${tahun}-${bulan}`));
    tampilkanData(dataTampil);
}

function tambahTransaksi() {
    const tanggal = document.getElementById("tanggal").value;
    const jenis = document.getElementById("jenis").value;
    const kategori = document.getElementById("kategori").value;
    const jumlah = Number(document.getElementById("jumlah").value);
    const keterangan = document.getElementById("keterangan").value;
    if (!tanggal || !jenis || !kategori || jumlah <= 0) return alert("Lengkapi semua data!");
    transaksi.push({ tanggal, jenis, kategori, jumlah, keterangan });
    localStorage.setItem("dataKas", JSON.stringify(transaksi));
    dataTampil = [...transaksi];
    ["tanggal","jenis","kategori","jumlah","keterangan"].forEach(id => document.getElementById(id).value = "");
    tampilkanData();
    alert("Transaksi disimpan!");
}

function cariTransaksi() {
    const kata = document.getElementById("kataKunci").value.toLowerCase();
    dataTampil = kata ? transaksi.filter(i => i.kategori.toLowerCase().includes(kata) || i.keterangan.toLowerCase().includes(kata)) : [...transaksi];
    tampilkanData(dataTampil);
}

function resetFilter() {
    document.getElementById("kataKunci").value = "";
    dataTampil = [...transaksi];
    tampilkanData();
}

function eksporKeCSV() {
    if (!dataTampil.length) return alert("Tidak ada data untuk diekspor!");
    let csv = "Tanggal,Jenis,Kategori,Jumlah,Keterangan\n";
    dataTampil.forEach(i => csv += `"${i.tanggal}","${i.jenis}","${i.kategori}","${i.jumlah}","${i.keterangan}"\n`);
    const blob = new Blob([csv], {type:"text/csv;charset=utf-8"});
    const a = document.createElement("a");
    a.href = URL.createObjectURL(blob);
    const bln = document.getElementById("pilihBulan").value;
    const thn = document.getElementById("pilihTahun").value;
    a.download = `Rekap_Kas_Bln${bln}_${thn}.csv`;
    a.click();
}
</script>

</body>
</html>
