# rekap-absensi-harian-smphq
<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Absensi Siswa Digital</title>

    <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf-autotable/3.5.25/jspdf.plugin.autotable.min.js"></script>

    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;500;600;700&display=swap" rel="stylesheet">

    <style>
        *{
            margin:0;
            padding:0;
            box-sizing:border-box;
            font-family:'Poppins',sans-serif;
        }

        body{
            background: linear-gradient(135deg,#2563eb,#06b6d4,#22c55e);
            min-height:100vh;
            padding:15px;
        }

        .container{
            max-width:1200px;
            margin:auto;
            background:rgba(255,255,255,0.95);
            backdrop-filter:blur(10px);
            border-radius:28px;
            padding:20px;
            box-shadow:0 10px 35px rgba(0,0,0,0.15);
        }

        .header{
            text-align:center;
            margin-bottom:25px;
        }

        .logo{
            width:70px;
            height:70px;
            margin:auto;
            border-radius:20px;
            background:linear-gradient(135deg,#2563eb,#22c55e);
            display:flex;
            align-items:center;
            justify-content:center;
            color:white;
            font-size:28px;
            font-weight:bold;
            margin-bottom:15px;
            box-shadow:0 5px 20px rgba(37,99,235,0.3);
        }

        .header h1{
            color:#1e3a8a;
            font-size:28px;
            font-weight:700;
        }

        .header p{
            color:#64748b;
            font-size:14px;
            margin-top:5px;
        }

        .form-box{
            background:#f8fafc;
            padding:20px;
            border-radius:22px;
            margin-bottom:25px;
        }

        /* GRID DISESUAIKAN UNTUK KOLOM KETERANGAN */
        .form-grid {
            display: grid;
            grid-template-columns: 1.5fr 0.8fr 1.2fr 0.8fr 1.5fr auto; 
            gap: 12px;
            align-items: end;
        }

        .input-group{
            display:flex;
            flex-direction:column;
            gap:7px;
        }

        .input-group label{
            font-size:13px;
            font-weight:600;
            color:#334155;
        }

        .form-grid input,
        .form-grid select{
            width:100%;
            padding:12px;
            border-radius:12px;
            border:1.5px solid #dbeafe;
            outline:none;
            font-size:14px;
            background:white;
        }

        .tanggal-box{
            display:flex;
            gap:5px;
        }
        
        .tanggal-box input {
            padding: 12px 2px !important;
            text-align: center;
        }

        #tgl, #bln { width: 55px; }
        #thn { width: 75px; }

        .rekap{
            display:grid;
            grid-template-columns:repeat(4, 1fr);
            gap:12px;
            margin-bottom:25px;
        }

        .card{
            padding:12px 15px;
            border-radius:18px;
            color:white;
            position:relative;
            overflow:hidden;
        }

        .card h4 { font-size: 12px; font-weight: 500; opacity: 0.9; }
        .card h2 { font-size: 24px; font-weight: 700; }

        .hadir{ background:linear-gradient(135deg,#22c55e,#16a34a); }
        .izin{ background:linear-gradient(135deg,#facc15,#eab308); }
        .sakit{ background:linear-gradient(135deg,#38bdf8,#2563eb); }
        .alfa{ background:linear-gradient(135deg,#fb7185,#dc2626); }

        .btn{ border:none; border-radius:12px; padding:12px 20px; cursor:pointer; font-weight:600; font-size:14px; transition: 0.3s; }
        .btn-primary{ background:#2563eb; color:white; }
        .btn-success{ background:#16a34a; color:white; }
        .btn-danger{ background:#ef4444; color:white; padding:6px 10px; border-radius:8px; font-size:12px; border:none; }

        .table-box{ background:#f8fafc; border-radius:20px; overflow:hidden; padding:10px; }
        .table-container{ overflow-x:auto; }
        table{ width:100%; border-collapse:collapse; min-width:900px; }
        th{ background:#2563eb; color:white; padding:14px; text-align:left; font-size: 14px; }
        td{ padding:12px; border-bottom:1px solid #e2e8f0; font-size:13px; }
        
        .status{ padding:4px 10px; border-radius:20px; font-size:10px; color:white; font-weight:600; text-transform: uppercase; }
        .status-hadir{ background:#16a34a; }
        .status-izin{ background:#eab308; }
        .status-sakit{ background:#2563eb; }
        .status-alfa{ background:#dc2626; }

        .footer{ margin-top:20px; display:flex; justify-content:space-between; align-items:center; }

        @media(max-width:1100px){
            .form-grid { grid-template-columns: 1fr 1fr; }
        }

        @media(max-width:600px){
            .form-grid { grid-template-columns: 1fr; }
            .rekap { grid-template-columns: repeat(2, 1fr); }
            .footer { flex-direction: column; gap: 15px; }
            #tgl, #bln, #thn { width: 100%; }
        }
    </style>
</head>
<body>

<div class="container">
    <div class="header">
        <div class="logo">A</div>
        <h1>ABSENSI DIGITAL SISWA <br> SMP HAMALATUL QURAN RINGINAGUNG</h1>
        <p>Kepung - Kediri</p>
    </div>

    <div class="form-box">
        <div class="form-grid">
            <div class="input-group">
                <label>Nama Siswa</label>
                <input type="text" id="nama" placeholder="Nama lengkap">
            </div>
            <div class="input-group">
                <label>Kelas</label>
                <select id="kelas">
                    <option value="">Pilih Kelas</option>
                    <option>Kelas 7</option>
                    <option>Kelas 8</option>
                    <option>Kelas 9</option>
                </select>
            </div>
            <div class="input-group">
                <label>Tanggal</label>
                <div class="tanggal-box">
                    <input type="number" id="tgl" placeholder="Tgl">
                    <input type="number" id="bln" placeholder="Bln">
                    <input type="number" id="thn" placeholder="Thn">
                </div>
            </div>
            <div class="input-group">
                <label>Status</label>
                <select id="status">
                    <option>Hadir</option>
                    <option>Izin</option>
                    <option>Sakit</option>
                    <option>Alfa</option>
                </select>
            </div>
            <div class="input-group">
                <label>Keterangan</label>
                <input type="text" id="keterangan" placeholder="Catatan tambahan">
            </div>
            <button class="btn btn-primary" onclick="tambahData()">Tambah</button>
        </div>
    </div>

    <div class="rekap">
        <div class="card hadir"><h4>Hadir</h4><h2 id="countHadir">0</h2></div>
        <div class="card izin"><h4>Izin</h4><h2 id="countIzin">0</h2></div>
        <div class="card sakit"><h4>Sakit</h4><h2 id="countSakit">0</h2></div>
        <div class="card alfa"><h4>Alfa</h4><h2 id="countAlfa">0</h2></div>
    </div>

    <div class="table-box">
        <div class="table-container">
            <table id="tableAbsensi">
                <thead>
                    <tr>
                        <th>No</th>
                        <th>Nama</th>
                        <th>Kelas</th>
                        <th>Hari</th>
                        <th>Tanggal</th>
                        <th>Status</th>
                        <th>Keterangan</th>
                        <th>Aksi</th>
                    </tr>
                </thead>
                <tbody id="tbody"></tbody>
            </table>
        </div>
    </div>

    <div class="footer">
        <h3>Total Data: <span id="total">0</span></h3>
        <button class="btn btn-success" onclick="downloadPDF()">Download PDF</button>
    </div>
</div>

<script>
let counter = 1;

function tambahData(){
    const nama = document.getElementById("nama").value;
    const kelas = document.getElementById("kelas").value;
    const tgl = document.getElementById("tgl").value;
    const bln = document.getElementById("bln").value;
    const thn = document.getElementById("thn").value;
    const status = document.getElementById("status").value;
    const keterangan = document.getElementById("keterangan").value || "-";

    if(!nama || !kelas || !tgl || !bln || !thn){
        alert("Harap isi semua kolom wajib!");
        return;
    }

    const dateObj = new Date(`${thn}-${bln}-${tgl}`);
    const hariList = ["Minggu","Senin","Selasa","Rabu","Kamis","Jumat","Sabtu"];
    const hari = hariList[dateObj.getDay()];
    const formatTanggal = String(tgl).padStart(2,'0') + "-" + String(bln).padStart(2,'0') + "-" + thn;

    updateRekap(status, 1);

    const tbody = document.getElementById("tbody");
    const row = tbody.insertRow();
    row.innerHTML = `
        <td>${counter++}</td>
        <td>${nama}</td>
        <td>${kelas}</td>
        <td>${hari}</td>
        <td>${formatTanggal}</td>
        <td><span class="status status-${status.toLowerCase()}">${status}</span></td>
        <td>${keterangan}</td>
        <td><button class="btn-danger" onclick="hapusBaris(this,'${status}')">Hapus</button></td>
    `;

    document.getElementById("total").innerText = tbody.rows.length;
    resetForm();
}

function updateRekap(status, val){
    const id = "count" + status;
    const el = document.getElementById(id);
    el.innerText = parseInt(el.innerText) + val;
}

function hapusBaris(btn, status){
    if(confirm("Hapus data ini?")){
        btn.parentElement.parentElement.remove();
        updateRekap(status, -1);
        document.getElementById("total").innerText = document.getElementById("tbody").rows.length;
    }
}

function resetForm(){
    document.getElementById("nama").value = "";
    document.getElementById("kelas").value = "";
    document.getElementById("tgl").value = "";
    document.getElementById("bln").value = "";
    document.getElementById("thn").value = "";
    document.getElementById("status").value = "Hadir";
    document.getElementById("keterangan").value = "";
}

function downloadPDF(){
    const { jsPDF } = window.jspdf;
    const doc = new jsPDF();
    doc.text("LAPORAN ABSENSI SISWA - SMP HAMALATUL QURAN", 14, 15);
    doc.setFontSize(10);
    doc.text("Tanggal Cetak: " + new Date().toLocaleDateString('id-ID'), 14, 22);
    
    doc.autoTable({
        html:'#tableAbsensi',
        startY: 30,
        didParseCell: (data) => { 
            // Jangan cetak kolom aksi (tombol hapus) di PDF
            if(data.column.index === 7) data.cell.text = ''; 
        }
    });
    doc.save("Absensi_Siswa.pdf");
}
</script>

</body>
</html>
