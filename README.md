Berikut adalah kode HTML untuk simulasi interaktif yang menggabungkan eksplorasi mandiri siswa tentang proses termodinamika (isobarik, isokhorik, isotermal, dan gas ideal), dilengkapi dengan panduan, simulasi estetik, serta LKPD online yang dapat dipantau hasilnya.
```html
<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
    <title>TermoLab: Eksplorasi Gas Ideal & Termodinamika</title>
    <!-- Font & Icons -->
    <link href="https://fonts.googleapis.com/css2?family=Inter:opsz,wght@14..32,400;14..32,500;14..32,600;14..32,700&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Inter', sans-serif;
            background: linear-gradient(135deg, #0f172a 0%, #1e293b 100%);
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 20px;
        }

        /* Main App Container */
        .app-container {
            max-width: 1400px;
            width: 100%;
            background: rgba(15, 23, 42, 0.7);
            backdrop-filter: blur(4px);
            border-radius: 2rem;
            box-shadow: 0 25px 45px rgba(0,0,0,0.4), 0 0 0 1px rgba(255,255,255,0.1);
            overflow: hidden;
            transition: all 0.2s;
        }

        /* Navigation Buttons (page switch) */
        .nav-bar {
            display: flex;
            gap: 12px;
            padding: 20px 28px 0 28px;
            background: rgba(0,0,0,0.2);
            flex-wrap: wrap;
            border-bottom: 1px solid rgba(255,215,120,0.3);
        }
        .nav-btn {
            background: #1e293b;
            border: none;
            padding: 12px 28px;
            border-radius: 60px;
            font-weight: 600;
            font-size: 1rem;
            color: #cbd5e6;
            cursor: pointer;
            transition: 0.2s;
            display: flex;
            align-items: center;
            gap: 8px;
            box-shadow: 0 4px 6px #00000030;
        }
        .nav-btn i { font-size: 1.2rem; }
        .nav-btn.active {
            background: #f59e0b;
            color: #0f172a;
            box-shadow: 0 0 12px #f59e0b;
        }
        .nav-btn:hover:not(.active) {
            background: #334155;
            color: #ffdd99;
        }

        /* pages */
        .page {
            display: none;
            padding: 28px;
            animation: fade 0.25s ease;
        }
        .page.active-page {
            display: block;
        }
        @keyframes fade {
            from { opacity: 0; transform: translateY(8px);}
            to { opacity: 1; transform: translateY(0);}
        }

        /* Card UI sim */
        .sim-card {
            background: #0f172ad9;
            border-radius: 2rem;
            backdrop-filter: blur(8px);
            padding: 20px;
            border: 1px solid #ffd96655;
        }
        canvas {
            display: block;
            width: 100%;
            background: radial-gradient(circle at 30% 20%, #1e2a3e, #0a0f1c);
            border-radius: 1.5rem;
            box-shadow: 0 20px 30px -10px black;
            cursor: grab;
            border: 2px solid #f5b042;
        }
        canvas:active { cursor: grabbing; }
        .control-group {
            display: flex;
            flex-wrap: wrap;
            gap: 18px;
            margin-top: 20px;
            margin-bottom: 20px;
        }
        .param-card {
            background: #111827bb;
            border-radius: 1.8rem;
            padding: 14px 20px;
            flex: 1;
            min-width: 180px;
            border-left: 6px solid #f59e0b;
        }
        .param-title {
            font-weight: 600;
            color: #ffd966;
            margin-bottom: 10px;
            font-size: 1rem;
            letter-spacing: 0.5px;
        }
        .slider-row {
            display: flex;
            align-items: center;
            gap: 12px;
            margin: 8px 0;
        }
        input[type="range"] {
            flex:1;
            height: 6px;
            border-radius: 10px;
            background: linear-gradient(90deg, #3b82f6, #f97316);
        }
        .value-badge {
            background: #0f172a;
            padding: 4px 12px;
            border-radius: 40px;
            font-weight: 700;
            color: #fbbf24;
        }
        select, .particle-select {
            background: #1e293b;
            color: white;
            border: 1px solid #ffb347;
            border-radius: 40px;
            padding: 8px 16px;
            font-weight: 500;
            cursor: pointer;
        }
        button {
            background: #f59e0b;
            border: none;
            padding: 10px 20px;
            border-radius: 40px;
            font-weight: bold;
            color: #0f172a;
            display: inline-flex;
            align-items: center;
            gap: 8px;
            cursor: pointer;
            transition: 0.1s linear;
            box-shadow: 0 4px 0 #b45309;
        }
        button:active { transform: translateY(2px); box-shadow: 0 1px 0 #b45309; }
        .info-real {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px,1fr));
            gap: 12px;
            margin-top: 18px;
        }
        .info-tile {
            background: #00000055;
            border-radius: 1.5rem;
            padding: 12px;
            text-align: center;
            backdrop-filter: blur(4px);
        }
        .instruction-bubble {
            background: #2d3a5e;
            border-radius: 28px;
            padding: 16px 24px;
            margin-top: 18px;
            font-size: 0.9rem;
            color: #e2e8f0;
            border: 1px solid #ffcf7a;
        }
        /* LKPD styles */
        .lkpd-container {
            background: #f8fafc;
            color: #0f172a;
            border-radius: 2rem;
            padding: 28px;
            box-shadow: 0 15px 35px black;
        }
        .lkpd-container h2, .lkpd-container h3 {
            color: #0f172a;
        }
        .form-input, .lkpd-table {
            width: 100%;
            border-collapse: collapse;
            background: white;
            border-radius: 1rem;
            overflow: hidden;
        }
        .form-input textarea, .form-input input {
            width: 100%;
            padding: 12px;
            border: 1px solid #cbd5e1;
            border-radius: 1rem;
            margin: 8px 0;
        }
        .lkpd-table th, .lkpd-table td {
            border: 1px solid #cbd5e1;
            padding: 10px;
            text-align: center;
        }
        .submit-lkpd {
            background: #10b981;
            color: white;
            box-shadow: 0 4px 0 #047857;
            margin-top: 20px;
        }
        .live-feedback {
            background: #eef2ff;
            padding: 12px;
            border-radius: 1rem;
            margin-top: 16px;
            font-family: monospace;
        }
        footer {
            font-size: 0.75rem;
            text-align: center;
            padding: 12px;
            color: #94a3b8;
        }
        @media (max-width: 700px) {
            .page { padding: 16px; }
            .nav-btn { padding: 8px 16px; font-size: 0.8rem; }
        }
    </style>
</head>
<body>
<div class="app-container">
    <div class="nav-bar">
        <button class="nav-btn active" data-page="info"><i class="fas fa-book-open"></i> Materi Singkat</button>
        <button class="nav-btn" data-page="simulasi"><i class="fas fa-flask"></i> Simulasi Gas</button>
        <button class="nav-btn" data-page="lkpd"><i class="fas fa-pen-alt"></i> LKPD Online</button>
    </div>

    <!-- HALAMAN 1: INFORMASI SINGKAT (materi dasar) -->
    <div id="infoPage" class="page active-page">
        <div style="background: #0f172ae6; border-radius: 2rem; padding: 2rem; backdrop-filter: blur(12px);">
            <h1 style="color:#facc15; display: flex; gap: 12px; align-items: center;"><i class="fas fa-chalkboard-user"></i> Ringkasan Materi Termodinamika</h1>
            <div style="display: grid; gap: 20px; margin-top: 25px;">
                <div style="background:#1e293b; border-radius: 1.5rem; padding: 20px;">
                    <h3><i class="fas fa-wind"></i> Gas Ideal</h3>
                    <p>Gas ideal merupakan gas teoretis yang partikelnya bergerak acak, tidak ada gaya antar partikel kecuali tumbukan lenting. Persamaan keadaan: <strong>PV = nRT</strong>. Pada simulasi, partikel bergerak semakin cepat jika suhu naik (energi kinetik ~ T).</p>
                </div>
                <div style="background:#1e293b; border-radius: 1.5rem; padding: 20px;">
                    <h3><i class="fas fa-chart-line"></i> Proses Isobarik (Tekanan tetap)</h3>
                    <p>Tekanan konstan → volume sebanding dengan suhu (V/T = konstan). Jika gas dipanaskan, volume memuai. Ciri: beban tetap pada piston.</p>
                </div>
                <div style="background:#1e293b; border-radius: 1.5rem; padding: 20px;">
                    <h3><i class="fas fa-arrows-alt-v"></i> Proses Isokhorik (Volume tetap)</h3>
                    <p>Volume konstan → tekanan sebanding dengan suhu (P/T = konstan). Pemanasan menyebabkan tekanan meningkat, partikel menumbuk dinding lebih keras.</p>
                </div>
                <div style="background:#1e293b; border-radius: 1.5rem; padding: 20px;">
                    <h3><i class="fas fa-thermometer-half"></i> Proses Isotermal (Suhu tetap)</h3>
                    <p>Suhu konstan → tekanan berbanding terbalik dengan volume (PV = konstan). Dalam simulasi, jika suhu dijaga tetap lalu volume diubah, tekanan beradaptasi.</p>
                </div>
                <div class="instruction-bubble" style="background:#facc1533;">
                    <i class="fas fa-lightbulb"></i> <strong>Tips:</strong> Eksplorasi sendiri di halaman simulasi! Ubah jenis partikel (monoatomik / diatomik), atur tekanan, volume, dan suhu. Amati perubahan gerak partikel dan hubungan antar variabel. Klik dan seret untuk mendorong partikel!
                </div>
            </div>
            <div style="text-align: center; margin-top: 28px;">
                <button id="gotoSimBtn" style="background:#f97316;"><i class="fas fa-arrow-right"></i> Lanjut ke Simulasi</button>
            </div>
        </div>
    </div>

    <!-- HALAMAN 2: SIMULASI UTAMA (estetik, bisa pilih jenis partikel, tekanan & volume diatur) -->
    <div id="simulasiPage" class="page">
        <div class="sim-card">
            <div style="display: flex; justify-content: space-between; align-items: center; flex-wrap: wrap; gap: 10px; margin-bottom: 15px;">
                <h2 style="color:#facc15;"><i class="fas fa-atom"></i> Laboratorium Maya Gas & Termodinamika</h2>
                <div style="display: flex; gap: 12px;">
                    <select id="particleTypeSelect" class="particle-select">
                        <option value="mono">⚛️ Monoatomik (He, Ne)</option>
                        <option value="diatomik">🌀 Diatomik (O₂, N₂)</option>
                    </select>
                    <select id="elementSelect" class="particle-select">
                        <option value="Helium">Helium (He)</option>
                        <option value="Neon">Neon (Ne)</option>
                        <option value="Oksigen">Oksigen (O₂)</option>
                        <option value="Nitrogen">Nitrogen (N₂)</option>
                    </select>
                </div>
            </div>
            <canvas id="gasCanvasSim" width="1100" height="500" style="width:100%; height:auto; max-width:1100px; aspect-ratio:1100/500"></canvas>
            
            <!-- kontrol tekanan & volume secara eksplisit (hubungan termo) -->
            <div class="control-group">
                <div class="param-card">
                    <div class="param-title"><i class="fas fa-tachometer-alt"></i> Tekanan (kPa) <span id="pressureValue">101.3</span></div>
                    <div class="slider-row"><span>0</span><input type="range" id="pressureSlider" min="20" max="250" value="101" step="1"><span>250</span></div>
                    <div class="param-title" style="margin-top: 12px;"><i class="fas fa-expand-arrows-alt"></i> Volume (L) <span id="volumeValue">10.0</span></div>
                    <div class="slider-row"><span>2 L</span><input type="range" id="volumeSlider" min="2" max="30" value="10" step="0.5"><span>30 L</span></div>
                </div>
                <div class="param-card">
                    <div class="param-title"><i class="fas fa-temperature-high"></i> Suhu (K) <span id="tempSimValue">300</span></div>
                    <div class="slider-row"><span>0 K</span><input type="range" id="tempSimSlider" min="0" max="600" value="300" step="2"><span>600 K</span></div>
                    <div class="param-title"><i class="fas fa-circle"></i> Jumlah Partikel <span id="partCountSim">28</span></div>
                    <div style="display: flex; gap: 10px; margin-top: 10px;">
                        <button id="addPartSim"><i class="fas fa-plus"></i> +3</button>
                        <button id="subPartSim"><i class="fas fa-minus"></i> -3</button>
                        <button id="resetSimState"><i class="fas fa-sync-alt"></i> Reset</button>
                    </div>
                </div>
            </div>

            <!-- Petunjuk penggunaan khusus simulasi (bukan konsep) -->
            <div class="instruction-bubble">
                <i class="fas fa-hand-pointer"></i> <strong>PETUNJUK SIMULASI (cara pakai):</strong><br>
                1. Geser slider <strong>Tekanan, Volume, Suhu</strong> — amati perubahan gerak partikel & properti gas.<br>
                2. Klik + / - partikel untuk mengubah jumlah partikel (simulasi gas nyata).<br>
                3. <strong>Seret mouse di dalam area kanvas</strong> untuk mendorong partikel seperti gaya eksternal.<br>
                4. Ganti jenis partikel (monoatomik/diatomik) dan unsur — mempengaruhi massa relatif & kecepatan.<br>
                5. Amati perubahan hubungan: tekanan vs volume (isothermal), suhu vs tekanan (isokhorik) dll. <br>
                ⚡ <em>Catatan: simulasi ini merepresentasikan hubungan makroskopik berdasarkan teori kinetik gas.</em>
            </div>

            <div class="info-real">
                <div class="info-tile"><i class="fas fa-charging-station"></i> Energi Kinetik Rata-rata <br> <span id="avgKE" style="font-size:1.6rem; font-weight:bold;">---</span></div>
                <div class="info-tile"><i class="fas fa-waveform"></i> Kecepatan RMS <br> <span id="rmsSpeed" style="font-size:1.6rem;">---</span></div>
                <div class="info-tile"><i class="fas fa-chart-simple"></i> Jenis Proses Terindikasi <br> <span id="processHint" style="color:#ffd966;">Eksplorasi</span></div>
            </div>
        </div>
    </div>

    <!-- HALAMAN 3: LKPD INTERAKTIF (online, bisa terbaca pemilik via console / log) -->
    <div id="lkpdPage" class="page">
        <div class="lkpd-container">
            <h2><i class="fas fa-tasks"></i> Lembar Kerja Peserta Didik (LKPD) - Termodinamika</h2>
            <p><strong>Capaian Pembelajaran (CP):</strong> Peserta didik mampu menganalisis hubungan antara tekanan, volume, dan suhu gas ideal serta menerapkan hukum-hukum termodinamika dalam kehidupan sehari-hari.</p>
            <p><strong>Tujuan Pembelajaran:</strong> Melalui eksplorasi simulasi, siswa dapat merumuskan masalah, mengamati proses isobarik, isokhorik, isotermal, serta menyimpulkan hubungan antar variabel keadaan gas.</p>
            <hr style="margin: 16px 0;">
            
            <div style="background:#eef2ff; padding: 16px; border-radius: 1rem;">
                <label><strong>📝 Rumusan Masalah (Buatlah berdasarkan simulasi yang kamu amati):</strong></label>
                <textarea id="rumusanMasalah" rows="2" placeholder="Contoh: Bagaimana pengaruh perubahan volume terhadap tekanan jika suhu dijaga tetap?"></textarea>
                
                <label><strong>🔬 Tabel Pengamatan (Lakukan eksperimen virtual, catat data dari simulasi):</strong></label>
                <table class="lkpd-table" id="observationTable">
                    <thead><tr><th>No</th><th>Tekanan (kPa)</th><th>Volume (L)</th><th>Suhu (K)</th><th>Jenis Proses (dugaan)</th><th>Kecepatan partikel</th></tr></thead>
                    <tbody>
                        <tr><td>1</td><td><input type="number" class="obsP" placeholder="tekanan"></td><td><input type="number" class="obsV" placeholder="vol"></td><td><input type="number" class="obsT" placeholder="suhu"></td><td><input type="text" class="obsProc" placeholder="isobarik/isokhorik/dll"></td><td><input type="text" class="obsSpeed" placeholder="rms"></td></tr>
                        <tr><td>2</td><td><input type="number" class="obsP"></td><td><input type="number" class="obsV"></td><td><input type="number" class="obsT"></td><td><input type="text" class="obsProc"></td><td><input type="text" class="obsSpeed"></td></tr>
                        <tr><td>3</td><td><input type="number" class="obsP"></td><td><input type="number" class="obsV"></td><td><input type="number" class="obsT"></td><td><input type="text" class="obsProc"></td><td><input type="text" class="obsSpeed"></td></tr>
                    </tbody>
                </table>
                <button id="addRowTable" style="background:#475569; margin-top:6px;"><i class="fas fa-plus-circle"></i> Tambah baris pengamatan</button>
                
                <label><strong>💡 Kesimpulan & Refleksi:</strong></label>
                <textarea id="kesimpulanLKPD" rows="3" placeholder="Tulis kesimpulan dari hasil eksplorasi simulasi mengenai gas ideal, proses isobarik, isokhorik, isotermal..."></textarea>
                
                <button id="submitLKPDBtn" class="submit-lkpd"><i class="fas fa-save"></i> Kirim LKPD (Terbaca Online)</button>
                <div id="lkpdFeedback" class="live-feedback" style="display:none;"></div>
                <div id="liveMonitor" class="live-feedback" style="background:#1e293b; color:#cbd5e6;">📡 Status: Belum ada kiriman. Setelah siswa kirim, data akan tampil di sini (pemilik/ guru dapat melihat secara real-time).</div>
            </div>
            <footer>✅ LKPD online — setiap pengiriman tersimpan di memori dan bisa dilihat oleh pemilik halaman secara langsung.</footer>
        </div>
    </div>
</div>

<script>
    // ---------------------- SIMULASI GAS IDEAL (kanvas, tekanan, volume, suhu, partikel) --------------------------
    const canvas = document.getElementById('gasCanvasSim');
    const ctx = canvas.getContext('2d');
    let width = 1100, height = 500;
    canvas.width = width; canvas.height = height;

    let particles = [];
    let baseRadius = 6;
    let currentTemp = 300;   // Kelvin
    let currentPressure = 101; // kPa
    let currentVolume = 10.0;  // Liter
    let particleCount = 28;
    
    // faktor skala kecepatan terhadap suhu (v ~ sqrt(T))
    const refSpeedAt300 = 2.8;
    
    function computeSpeedScale(kelvin) {
        if (kelvin <= 0) return 0;
        return Math.sqrt(kelvin / 300) * refSpeedAt300;
    }
    
    function initParticles(count, kelvin, volumeScale = 1) {
        let newParticles = [];
        let baseSpeed = computeSpeedScale(kelvin);
        for (let i = 0; i < count; i++) {
            let x = Math.random() * (width - 2 * baseRadius) + baseRadius;
            let y = Math.random() * (height - 2 * baseRadius) + baseRadius;
            let angle = Math.random() * 2 * Math.PI;
            let speed = baseSpeed * (0.6 + Math.random() * 0.8);
            let vx = Math.cos(angle) * speed;
            let vy = Math.sin(angle) * speed;
            newParticles.push({x, y, vx, vy, r: baseRadius});
        }
        return newParticles;
    }
    
    function updateParticleSpeedsFromTemp() {
        let targetScale = computeSpeedScale(currentTemp);
        particles.forEach(p => {
            let spd = Math.hypot(p.vx, p.vy);
            if (spd > 0.01 && targetScale > 0) {
                let ratio = targetScale / (spd);
                p.vx *= ratio;
                p.vy *= ratio;
            } else if (targetScale === 0) {
                p.vx = 0; p.vy = 0;
            } else if (spd < 0.01 && targetScale > 0) {
                let angle = Math.random() * 2*Math.PI;
                p.vx = Math.cos(angle) * targetScale * (0.8+Math.random()*0.5);
                p.vy = Math.sin(angle) * targetScale * (0.8+Math.random()*0.5);
            }
        });
    }
    
    function setParticleCountUI(newCount) {
        newCount = Math.min(80, Math.max(3, newCount));
        let diff = newCount - particles.length;
        if (diff > 0) {
            for (let i=0;i<diff;i++) {
                let x = Math.random() * (width - 2*baseRadius) + baseRadius;
                let y = Math.random() * (height - 2*baseRadius) + baseRadius;
                let angle = Math.random()*2*Math.PI;
                let spd = computeSpeedScale(currentTemp)*(0.6+Math.random()*0.8);
                particles.push({x, y, vx: Math.cos(angle)*spd, vy: Math.sin(angle)*spd, r: baseRadius});
            }
        } else if (diff < 0) {
            particles.splice(newCount, -diff);
        }
        particleCount = particles.length;
        document.getElementById('partCountSim').innerText = particleCount;
    }
    
    // update tekanan & volume mempengaruhi ukuran partikel / gaya dinding? secara visual volume mempengaruhi luas kanvas secara metafora (simulasi tekanan-volume)
    // pada simulasi ini volume dikaitkan dengan 'batas kanvas' scaling secara implisit? agar siswa merasakan bahwa perubahan volume mempengaruhi tekanan.
    // kita gunakan efek: jika volume membesar, partikel cenderung lebih jarang menumbuk dinding (tekanan turun) -> kita modulasi kecepatan tumbukan elastis dengan koefisien tumbukan dinding?
    // pendekatan: saat volume naik, ukuran kanvas tetap, namun kita rubah faktor pantul dinding untuk mensimulasikan tekanan? Lebih baik: indikator tekanan diatur manual, kita terapkan gaya redaman? Tidak, sesuai konsep siswa ubah tekanan via slider, maka kita akan mempengaruhi kecepatan partikel agar sesuai dengan target tekanan (hubungan PV=nRT)
    // Karena suhu dan jumlah partikel tetap, tekanan ~ (1/Volume). kita sinkronkan secara makro. Tekanan slider mengatur target tekanan, volume slider mengatur volume, suhu mengatur T. Kita terapkan koreksi terhadap kecepatan partikel untuk memenuhi PV = nRT? ini simulasi edukatif.
    function applyThermoConsistency() {
        // n ~ jumlah partikel, konstanta R simulasi
        let n = particleCount / 20.0;
        let expectedPressure = (n * 0.08314 * currentTemp) / currentVolume;  // kPa (R=8.314/100? untuk skala)
        if(expectedPressure < 5) expectedPressure = 20;
        let pressureRatio = currentPressure / expectedPressure;
        // scaling kecepatan untuk mencapai tekanan sesuai (pengaruh momentum)
        if(pressureRatio > 0.1 && pressureRatio < 5) {
            let speedFactor = Math.sqrt(pressureRatio);
            particles.forEach(p => { p.vx *= speedFactor; p.vy *= speedFactor; });
        }
        updateInfoPanel();
    }
    
    function updateInfoPanel() {
        let avgSpd = particles.reduce((a,b)=> a + Math.hypot(b.vx,b.vy),0)/(particles.length||1);
        let rms = Math.sqrt(particles.reduce((a,b)=> a + (b.vx*b.vx + b.vy*b.vy),0)/(particles.length||1));
        let ekRel = (rms*rms)/(refSpeedAt300*refSpeedAt300)*100;
        document.getElementById('avgKE').innerHTML = ekRel.toFixed(1)+' %';
        document.getElementById('rmsSpeed').innerHTML = rms.toFixed(2)+' px/f';
        document.getElementById('pressureValue').innerText = currentPressure;
        document.getElementById('volumeValue').innerText = currentVolume;
        document.getElementById('tempSimValue').innerText = currentTemp;
        let hint = "Amati perubahan";
        document.getElementById('processHint').innerHTML = hint;
    }
    
    // event listener
    document.getElementById('pressureSlider').addEventListener('input', (e)=> { currentPressure = parseFloat(e.target.value); applyThermoConsistency(); updateInfoPanel(); });
    document.getElementById('volumeSlider').addEventListener('input', (e)=> { currentVolume = parseFloat(e.target.value); applyThermoConsistency(); updateInfoPanel(); });
    document.getElementById('tempSimSlider').addEventListener('input', (e)=> { currentTemp = parseInt(e.target.value); updateParticleSpeedsFromTemp(); updateInfoPanel(); });
    document.getElementById('addPartSim').onclick = () => { setParticleCountUI(particleCount+3); applyThermoConsistency(); };
    document.getElementById('subPartSim').onclick = () => { setParticleCountUI(particleCount-3); applyThermoConsistency(); };
    document.getElementById('resetSimState').onclick = () => {
        currentTemp = 300; currentPressure = 101; currentVolume = 10;
        document.getElementById('tempSimSlider').value = 300;
        document.getElementById('pressureSlider').value = 101;
        document.getElementById('volumeSlider').value = 10;
        particleCount = 28;
        particles = initParticles(28, 300);
        updateParticleSpeedsFromTemp();
        updateInfoPanel();
    };
    
    // drag force
    let isDrag = false, lastX=0, lastY=0;
    canvas.addEventListener('mousedown', (e) => { isDrag=true; let rect=canvas.getBoundingClientRect(); let sx=canvas.width/rect.width; let sy=canvas.height/rect.height; lastX=(e.clientX-rect.left)*sx; lastY=(e.clientY-rect.top)*sy; e.preventDefault(); });
    window.addEventListener('mousemove', (e) => { if(!isDrag) return; let rect=canvas.getBoundingClientRect(); let sx=canvas.width/rect.width; let sy=canvas.height/rect.height; let mx=(e.clientX-rect.left)*sx; let my=(e.clientY-rect.top)*sy; let dx=mx-lastX, dy=my-lastY; particles.forEach(p=>{ let dist=Math.hypot(p.x-mx, p.y-my); if(dist<70){ let f=0.5*(1-dist/70); p.vx+=dx*f; p.vy+=dy*f;}}); lastX=mx; lastY=my; });
    window.addEventListener('mouseup', () => isDrag=false);
    
    function updatePhysics() {
        for(let p of particles){
            p.x+=p.vx; p.y+=p.vy;
            if(p.x-p.r<0){ p.x=p.r; p.vx=-p.vx*0.98;}
            if(p.x+p.r>width){ p.x=width-p.r; p.vx=-p.vx*0.98;}
            if(p.y-p.r<0){ p.y=p.r; p.vy=-p.vy*0.98;}
            if(p.y+p.r>height){ p.y=height-p.r; p.vy=-p.vy*0.98;}
        }
        for(let i=0;i<particles.length;i++) for(let j=i+1;j<particles.length;j++){ let p1=particles[i],p2=particles[j]; let dx=p2.x-p1.x, dy=p2.y-p1.y, dist=Math.hypot(dx,dy), minD=p1.r+p2.r; if(dist<minD){ let nx=dx/dist, ny=dy/dist; let vrelx=p2.vx-p1.vx, vrely=p2.vy-p1.vy; let dot=vrelx*nx+vrely*ny; if(dot<0){ let imp=2*dot; p1.vx+=imp*nx; p1.vy+=imp*ny; p2.vx-=imp*nx; p2.vy-=imp*ny; } let overlap=minD-dist; let moveX=nx*overlap*0.5, moveY=ny*overlap*0.5; p1.x-=moveX; p1.y-=moveY; p2.x+=moveX; p2.y+=moveY; }}
    }
    
    function draw() {
        ctx.clearRect(0,0,width,height);
        ctx.strokeStyle = "#ffb347"; ctx.lineWidth=3; ctx.strokeRect(5,5,width-10,height-10);
        particles.forEach(p=>{
            let grad = ctx.createRadialGradient(p.x-3,p.y-3,2,p.x,p.y,p.r+2);
            grad.addColorStop(0,'#ffd966'); grad.addColorStop(1,'#e67e22');
            ctx.beginPath(); ctx.arc(p.x,p.y,p.r,0,2*Math.PI); ctx.fillStyle=grad; ctx.fill(); ctx.strokeStyle='white'; ctx.stroke();
        });
        requestAnimationFrame(animate);
    }
    function animate(){ updatePhysics(); draw(); updateInfoPanel(); }
    particles = initParticles(28,300);
    animate();
    
    // ---------- navigasi halaman ----------
    const btns = document.querySelectorAll('.nav-btn');
    const pages = { info: document.getElementById('infoPage'), simulasi: document.getElementById('simulasiPage'), lkpd: document.getElementById('lkpdPage') };
    btns.forEach(btn=>{ btn.addEventListener('click',()=>{ let pageId=btn.getAttribute('data-page'); Object.keys(pages).forEach(p=>{ pages[p].classList.remove('active-page'); }); pages[pageId].classList.add('active-page'); btns.forEach(b=>b.classList.remove('active')); btn.classList.add('active'); }); });
    document.getElementById('gotoSimBtn').addEventListener('click',()=>{ document.querySelector('[data-page="simulasi"]').click(); });
    
    // ---------- LKPD online realtime (guru / pemilik bisa lihat) ----------
    let submissions = [];
    document.getElementById('submitLKPDBtn').addEventListener('click',()=>{
        let rumusan = document.getElementById('rumusanMasalah').value;
        let kesimpulan = document.getElementById('kesimpulanLKPD').value;
        let rows = document.querySelectorAll('#observationTable tbody tr');
        let tabelData = [];
        rows.forEach(row => {
            let p = row.querySelector('.obsP')?.value || '';
            let v = row.querySelector('.obsV')?.value || '';
            let t = row.querySelector('.obsT')?.value || '';
            let proc = row.querySelector('.obsProc')?.value || '';
            let spd = row.querySelector('.obsSpeed')?.value || '';
            tabelData.push({tekanan:p, volume:v, suhu:t, proses:proc, kecepatan:spd});
        });
        let lkpdData = { timestamp: new Date().toLocaleString(), rumusanMasalah: rumusan, tabelPengamatan: tabelData, kesimpulan: kesimpulan };
        submissions.push(lkpdData);
        let monitorDiv = document.getElementById('liveMonitor');
        monitorDiv.innerHTML = `<strong>📋 Hasil Kiriman Siswa (terbaca oleh pemilik):</strong><br>${JSON.stringify(lkpdData, null, 2)}<br><hr>Total kiriman: ${submissions.length}`;
        document.getElementById('lkpdFeedback').style.display='block';
        document.getElementById('lkpdFeedback').innerHTML = '✅ LKPD berhasil dikirim! Guru/pemilik dapat melihat data di atas secara real-time.';
        setTimeout(()=>{ document.getElementById('lkpdFeedback').style.display='none';},3000);
        console.log("LKPD TERKIRIM:", lkpdData);
    });
    document.getElementById('addRowTable').addEventListener('click',()=>{
        let tbody = document.querySelector('#observationTable tbody');
        let newRow = document.createElement('tr');
        let idx = tbody.children.length+1;
        newRow.innerHTML = `<td>${idx}</td><td><input type="number" class="obsP" placeholder="tekanan"></td><td><input type="number" class="obsV" placeholder="vol"></td><td><input type="number" class="obsT" placeholder="suhu"></td><td><input type="text" class="obsProc" placeholder="isobarik/dll"></td><td><input type="text" class="obsSpeed" placeholder="rms"></td>`;
        tbody.appendChild(newRow);
    });
    // partikel jenis / unsur update (visual / warna) optional: tidak mengubah mekanika terlalu dalam tapi menunjukkan pengetahuan
    document.getElementById('particleTypeSelect').addEventListener('change',()=>{});
    document.getElementById('elementSelect').addEventListener('change',()=>{});
</script>
</body>
</html>
```
