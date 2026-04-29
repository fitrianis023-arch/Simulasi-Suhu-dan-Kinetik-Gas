<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
    <title>TermoLab | Simulasi Gas & LKPD Digital</title>
    <link href="https://fonts.googleapis.com/css2?family=Inter:opsz,wght@14..32,300;400;500;600;700;800&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css">
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; }
        body {
            background: radial-gradient(ellipse at 30% 40%, #0f172a, #020617);
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 24px;
            font-family: 'Inter', sans-serif;
        }
        .cosmic-container {
            width: 100%;
            max-width: 1400px;
            background: rgba(15, 25, 45, 0.55);
            backdrop-filter: blur(14px);
            border-radius: 3rem;
            border: 1px solid rgba(255, 255, 255, 0.2);
            box-shadow: 0 30px 50px rgba(0, 0, 0, 0.5);
            overflow: hidden;
        }
        .content-wrap { padding: 2rem 2.2rem; }
        button, .btn {
            cursor: pointer;
            border: none;
            font-weight: 600;
            transition: all 0.2s;
            background: rgba(255, 255, 255, 0.1);
            backdrop-filter: blur(8px);
            border-radius: 2rem;
            padding: 0.7rem 1.6rem;
            color: #f1f5f9;
            border: 0.5px solid rgba(255,255,200,0.3);
        }
        button:hover { transform: translateY(-3px); background: rgba(255, 255, 255, 0.2); }
        .btn-primary { background: linear-gradient(135deg, #2d6a4f, #1b4d3e); border: none; }
        
        .lkpd-menu-card {
            flex: 1;
            min-width: 220px;
            text-align: center;
            padding: 2rem 1.5rem;
            border-radius: 2rem;
            cursor: pointer;
            transition: all 0.3s;
        }
        .lkpd-menu-card.volume-tetap { background: linear-gradient(145deg, #064e3b, #022c22); border-top: 4px solid #10b981; }
        .lkpd-menu-card.tekanan-tetap { background: linear-gradient(145deg, #1e3a5f, #0f2a4a); border-top: 4px solid #3b82f6; }
        .lkpd-menu-card.suhu-tetap { background: linear-gradient(145deg, #78350f, #451a03); border-top: 4px solid #f59e0b; }
        .lkpd-menu-card:hover { transform: translateY(-8px); filter: brightness(1.05); }
        .lkpd-menu-card i { font-size: 3rem; display: block; margin-bottom: 1rem; }
        .lkpd-menu-card h3 { color: #ffffff; font-size: 1.3rem; margin-bottom: 0.5rem; }
        .lkpd-menu-card .judul { font-size: 1rem; font-weight: 700; margin: 0.5rem 0; color: #facc15; }
        .lkpd-menu-card .sub { color: #cbd5e6; font-size: 0.8rem; }
        
        .lkpd-form-container {
            border-radius: 2rem;
            padding: 2rem;
        }
        .lkpd-form-container.volume-tetap { border: 2px solid rgba(16, 185, 129, 0.5); background: linear-gradient(145deg, #022c22, #011a12); }
        .lkpd-form-container.tekanan-tetap { border: 2px solid rgba(59, 130, 246, 0.5); background: linear-gradient(145deg, #0f2a4a, #071a30); }
        .lkpd-form-container.suhu-tetap { border: 2px solid rgba(245, 158, 11, 0.5); background: linear-gradient(145deg, #451a03, #2a0e01); }
        
        .lkpd-header { text-align: center; margin-bottom: 2rem; padding-bottom: 1rem; border-bottom: 2px dashed rgba(255,255,255,0.2); }
        .lkpd-header h2 { font-size: 1.5rem; font-weight: 700; color: #ffffff; margin-bottom: 0.75rem; }
        .badge { display: inline-block; padding: 0.35rem 1.2rem; border-radius: 2rem; font-size: 0.8rem; font-weight: 600; }
        .badge-volume { background: rgba(16, 185, 129, 0.3); color: #a7f3d0; border: 1px solid #10b981; }
        .badge-tekanan { background: rgba(59, 130, 246, 0.3); color: #bfdbfe; border: 1px solid #3b82f6; }
        .badge-suhu { background: rgba(245, 158, 11, 0.3); color: #fde68a; border: 1px solid #f59e0b; }
        
        .lkpd-section { background: rgba(0, 0, 0, 0.6); border-radius: 1rem; padding: 1.2rem; margin-bottom: 1.2rem; }
        .lkpd-section-title { font-size: 1rem; font-weight: 700; color: #facc15; margin-bottom: 0.75rem; display: flex; align-items: center; gap: 8px; }
        .tujuan-text { background: rgba(0, 0, 0, 0.5); border-radius: 0.8rem; padding: 12px 16px; color: #f1f5f9; font-size: 0.95rem; border-left: 4px solid #facc15; line-height: 1.5; }
        .tujuan-text strong { color: #facc15; }
        .lkpd-input, .lkpd-textarea { width: 100%; background: rgba(0, 0, 0, 0.7); border: 1px solid rgba(255,255,255,0.2); border-radius: 0.8rem; padding: 10px 14px; color: #ffffff; font-size: 0.9rem; }
        .lkpd-input:focus, .lkpd-textarea:focus { outline: none; border-color: #facc15; }
        
        .observ-table {
            width: 100%;
            border-collapse: collapse;
            font-size: 0.85rem;
            border-radius: 0.8rem;
            overflow: hidden;
        }
        .observ-table th, .observ-table td {
            padding: 10px 8px;
            text-align: center;
            border: 1px solid rgba(255, 255, 255, 0.15);
        }
        .observ-table th {
            background: rgba(250, 204, 21, 0.25);
            color: #facc15;
            font-weight: 700;
        }
        .observ-table td { background: rgba(0, 0, 0, 0.5); }
        .observ-table input {
            width: 100%;
            background: transparent;
            border: none;
            color: #ffffff;
            text-align: center;
            font-size: 0.85rem;
            padding: 6px 0;
        }
        .observ-table input:focus { outline: none; color: #facc15; }
        .observ-table input::placeholder { color: #64748b; font-size: 0.7rem; }
        .observ-table input[readonly] { color: #94a3b8; font-style: italic; }
        .tetap-badge {
            display: inline-block;
            background: rgba(250, 204, 21, 0.2);
            padding: 2px 6px;
            border-radius: 4px;
            font-size: 0.65rem;
            color: #facc15;
            margin-left: 5px;
        }
        
        .save-btn { background: linear-gradient(135deg, #2d6a4f, #1b4d3e); color: white; padding: 12px; font-weight: 600; border-radius: 2rem; width: 100%; display: flex; align-items: center; justify-content: center; gap: 10px; cursor: pointer; border: none; margin-top: 0.5rem; }
        .save-btn:hover { background: linear-gradient(135deg, #40916c, #2d6a4f); transform: translateY(-2px); }
        .feedback-guru { background: rgba(0, 0, 0, 0.5); border-radius: 1rem; padding: 0.8rem; margin-top: 1rem; display: flex; align-items: center; gap: 10px; border-left: 4px solid #facc15; font-size: 0.75rem; color: #cbd5e6; }
        .info-text { font-size: 0.7rem; color: #94a3b8; margin-top: 8px; display: flex; align-items: center; gap: 6px; }
        .nav-buttons { display: flex; justify-content: space-between; align-items: center; flex-wrap: wrap; gap: 1rem; margin-bottom: 1rem; }
        
        /* Simulasi */
        .sim-card { background: rgba(18, 28, 40, 0.8); border-radius: 2rem; padding: 1.5rem; margin: 1rem 0; }
        .canvas-orb { display: flex; justify-content: center; background: rgba(0,0,0,0.3); border-radius: 2rem; padding: 12px; margin-bottom: 1.5rem; }
        canvas { background: radial-gradient(circle at 30% 20%, #1e293b, #0f172a); border-radius: 2rem; width: 100%; height: auto; }
        .param-pod { background: rgba(0,0,0,0.5); border-radius: 1.5rem; padding: 1rem; flex: 1; min-width: 150px; }
        .param-pod label { color: #facc15; font-weight: 600; display: flex; align-items: center; gap: 8px; margin-bottom: 8px; }
        input[type="range"] { width: 100%; height: 4px; border-radius: 10px; background: #334155; -webkit-appearance: none; }
        input[type="range"]:disabled { opacity: 0.4; cursor: not-allowed; }
        input[type="range"]::-webkit-slider-thumb { width: 16px; height: 16px; border-radius: 50%; background: #facc15; cursor: pointer; }
        input[type="range"]:disabled::-webkit-slider-thumb { background: #6b7280; cursor: not-allowed; }
        .value-badge { background: #0f172a; padding: 4px 10px; border-radius: 2rem; font-size: 0.8rem; font-weight: 500; color: #facc15; display: inline-block; margin-top: 6px; }
        
        /* Pilihan jenis partikel */
        .partikel-selector {
            background: rgba(0, 0, 0, 0.5);
            border-radius: 1.5rem;
            padding: 1rem;
            margin-top: 1rem;
            display: flex;
            flex-wrap: wrap;
            align-items: center;
            justify-content: center;
            gap: 1rem;
        }
        .partikel-selector label {
            color: #facc15;
            font-weight: 600;
            margin-right: 0.5rem;
        }
        .partikel-option {
            display: inline-flex;
            align-items: center;
            gap: 0.5rem;
            background: rgba(0, 0, 0, 0.4);
            padding: 0.4rem 1rem;
            border-radius: 2rem;
            cursor: pointer;
            transition: all 0.2s;
            border: 1px solid rgba(255,255,255,0.1);
        }
        .partikel-option:hover {
            background: rgba(250, 204, 21, 0.2);
            border-color: #facc15;
        }
        .partikel-option.active {
            background: rgba(250, 204, 21, 0.3);
            border-color: #facc15;
            box-shadow: 0 0 8px rgba(250,204,21,0.3);
        }
        .partikel-option input {
            margin-right: 4px;
            cursor: pointer;
        }
        .unsur-select {
            background: rgba(0, 0, 0, 0.6);
            border: 1px solid #facc15;
            border-radius: 2rem;
            padding: 0.4rem 1rem;
            color: #facc15;
            font-weight: 600;
            cursor: pointer;
            margin-left: 0.5rem;
        }
        .unsur-select option {
            background: #0f172a;
            color: #ffffff;
        }
        
        .molecular-info { display: flex; flex-wrap: wrap; gap: 1rem; margin: 1rem 0; }
        .info-card { flex: 1; background: rgba(0, 0, 0, 0.6); border-radius: 1rem; padding: 0.8rem; text-align: center; border: 1px solid rgba(250, 204, 21, 0.4); }
        .info-card i { font-size: 1.5rem; color: #facc15; margin-bottom: 5px; display: block; }
        .info-label { font-size: 0.7rem; text-transform: uppercase; color: #94a3b8; letter-spacing: 1px; }
        .info-value { font-size: 1.3rem; font-weight: 800; color: #facc15; }
        .info-unit { font-size: 0.65rem; color: #6b7280; margin-top: 2px; }
        
        .particle-controller { background: rgba(0,0,0,0.5); border-radius: 2rem; padding: 0.8rem 1.5rem; display: flex; align-items: center; justify-content: space-between; flex-wrap: wrap; gap: 1rem; margin-top: 1rem; }
        .particle-controller span { color: #facc15; font-weight: 600; }
        .counter-group { display: flex; gap: 0.8rem; align-items: center; }
        .counter-group button { background: #1e293b; font-size: 1.2rem; padding: 0.4rem 1.2rem; border-radius: 2rem; color: #facc15; cursor: pointer; }
        .particle-num { background: #00000066; padding: 0.3rem 1.5rem; border-radius: 2rem; font-size: 1.4rem; font-weight: 800; color: #facc15; }
        .proses-buttons { display: flex; gap: 0.8rem; justify-content: center; margin-bottom: 1rem; flex-wrap: wrap; }
        .btn-proses { background: rgba(0,0,0,0.6); padding: 0.6rem 1.2rem; border-radius: 2rem; font-size: 0.85rem; font-weight: 600; color: #e2e8f0; cursor: pointer; }
        .btn-proses.aktif { background: #2c5530; border-left: 3px solid #facc15; color: #facc15; }
        
        h2 { color: #ffffff; font-weight: 600; }
        p { color: #cbd5e6; }
        
        @media (max-width: 780px) { 
            .content-wrap { padding: 1rem; } 
            .observ-table th, .observ-table td { padding: 6px 4px; font-size: 0.7rem; }
            .observ-table input { font-size: 0.7rem; }
            .partikel-selector { flex-direction: column; align-items: stretch; }
        }
    </style>
</head>
<body>
<div class="cosmic-container" id="appRoot"></div>

<script>
    // ==================== DATA LKPD (TIDAK BERUBAH) ====================
    let lkpdData = {
        volume_tetap: { tujuan: "", rumusan: "", hipotesis: "", tabel: [], analisis: "", kesimpulan: "" },
        tekanan_tetap: { tujuan: "", rumusan: "", hipotesis: "", tabel: [], analisis: "", kesimpulan: "" },
        suhu_tetap: { tujuan: "", rumusan: "", hipotesis: "", tabel: [], analisis: "", kesimpulan: "" }
    };
    
    function loadLKPD() { 
        const saved = localStorage.getItem("termolab_lkpd_fix"); 
        if(saved) { 
            try { 
                let p = JSON.parse(saved); 
                if(p.volume_tetap) lkpdData = p; 
            } catch(e) {} 
        }
        for(let mode of ['volume_tetap', 'tekanan_tetap', 'suhu_tetap']) {
            if(!lkpdData[mode].tabel || lkpdData[mode].tabel.length === 0) {
                lkpdData[mode].tabel = [{}, {}, {}, {}, {}];
            }
        }
    }
    function saveLKPDLocal() { localStorage.setItem("termolab_lkpd_fix", JSON.stringify(lkpdData)); }
    loadLKPD();
    function escapeHtml(str) { if(!str) return ""; return str.replace(/[&<>]/g, m => ({ '&':'&amp;', '<':'&lt;', '>':'&gt;' }[m])); }
    
    let currentPage = "start";
    let lkpdMode = null;
    
    // ==================== SIMULASI GAS ====================
    let canvas, ctx, animationId = null;
    let particles = [];
    let PARTICLE_COUNT = 48;
    let containerWidth = 800, containerHeight = 400;
    let pressure = 2.5, volume = 3.0, temperature = 400;
    let activeProcess = 'tekanan_tetap';
    let refPressure = 2.5, refVolume = 3.0;
    
    // Jenis partikel dan unsur
    let partikelJenis = 'monoatomik'; // 'monoatomik' atau 'diatomik'
    let unsurTerpilih = 'He'; // default Helium
    
    // Data unsur
    const unsurMonoatomik = [
        { nama: 'Helium (He)', kode: 'He', massa: 6.64e-27, warna: '#ffb347', kecepatanFaktor: 1.0 },
        { nama: 'Neon (Ne)', kode: 'Ne', massa: 3.35e-26, warna: '#ff8c42', kecepatanFaktor: 0.95 },
        { nama: 'Argon (Ar)', kode: 'Ar', massa: 6.63e-26, warna: '#ff6b35', kecepatanFaktor: 0.88 }
    ];
    const unsurDiatomik = [
        { nama: 'Oksigen (O₂)', kode: 'O₂', massa: 5.31e-26, warna: '#6ab0de', kecepatanFaktor: 0.92 },
        { nama: 'Nitrogen (N₂)', kode: 'N₂', massa: 4.65e-26, warna: '#5a9ec2', kecepatanFaktor: 0.95 }
    ];
    
    let currentMassa = 6.64e-27; // massa Helium
    let currentWarna = '#ffb347';
    
    function updateMassaDanWarna() {
        if (partikelJenis === 'monoatomik') {
            const unsur = unsurMonoatomik.find(u => u.kode === unsurTerpilih) || unsurMonoatomik[0];
            currentMassa = unsur.massa;
            currentWarna = unsur.warna;
        } else {
            const unsur = unsurDiatomik.find(u => u.kode === unsurTerpilih) || unsurDiatomik[0];
            currentMassa = unsur.massa;
            currentWarna = unsur.warna;
        }
    }
    
    const MIN_VOL = 0.5, MAX_VOL = 10.0;
    const MIN_TEMP = 0, MAX_TEMP = 800;
    const MIN_PRESS = 0, MAX_PRESS = 5.0;
    const NORM_TEMP = 400, NORM_PRESS = 2.5, NORM_VOL = 3.0;
    const MIN_PARTICLES = 5, MAX_PARTICLES = 200;
    
    const BOLTZMANN = 1.38e-23;
    
    function calculateVolumeFromTempAndPressure(temp, press) {
        if(press <= 0) return MAX_VOL;
        return Math.min(MAX_VOL, Math.max(MIN_VOL, NORM_VOL * (temp / NORM_TEMP) * (NORM_PRESS / press)));
    }
    function calculatePressureFromTempAndVolume(temp, vol) {
        if(vol <= 0) return MAX_PRESS;
        return Math.min(MAX_PRESS, Math.max(MIN_PRESS, NORM_PRESS * (temp / NORM_TEMP) * (NORM_VOL / vol)));
    }
    
    function calculateAvgKineticEnergy() {
        let factor = (partikelJenis === 'monoatomik') ? 1.5 : 2.5;
        let ekJoules = factor * BOLTZMANN * Math.max(0.1, temperature);
        let scaled = ekJoules * 1e21 * 100;
        return Math.min(5000, Math.max(0, scaled)).toFixed(0);
    }
    
    function calculateRMSSpeed() {
        let rms = Math.sqrt((3 * BOLTZMANN * Math.max(0.1, temperature)) / currentMassa);
        return Math.round(rms);
    }
    
    function calculateCollisionsPerSecond() {
        let avgSpeed = calculateRMSSpeed();
        let numberDensity = PARTICLE_COUNT / Math.max(0.1, volume);
        let diameter = (partikelJenis === 'monoatomik') ? 2.2e-10 : 3.0e-10;
        let collFreq = numberDensity * Math.PI * diameter * diameter * avgSpeed * 1.414;
        let scaledColl = collFreq * 1e13;
        return Math.min(80000, Math.max(0, Math.round(scaledColl))).toLocaleString();
    }
    
    function updateMolecularInfo() {
        const ekSpan = document.getElementById('kineticEnergy');
        const speedSpan = document.getElementById('avgSpeed');
        const collSpan = document.getElementById('collisionRate');
        const jenisSpan = document.getElementById('jenisPartikel');
        if(ekSpan) ekSpan.innerText = calculateAvgKineticEnergy();
        if(speedSpan) speedSpan.innerText = calculateRMSSpeed();
        if(collSpan) collSpan.innerText = calculateCollisionsPerSecond();
        if(jenisSpan) jenisSpan.innerText = partikelJenis === 'monoatomik' ? 'Monoatomik' : 'Diatomik';
    }
    
    function updateByProcess() {
        let newPress = parseFloat(document.getElementById('pressSlider')?.value) || pressure;
        let newTemp = parseFloat(document.getElementById('tempSlider')?.value) || temperature;
        let newVol = parseFloat(document.getElementById('volSlider')?.value) || volume;
        
        if(activeProcess === 'tekanan_tetap') {
            pressure = newPress;
            temperature = newTemp;
            volume = calculateVolumeFromTempAndPressure(temperature, pressure);
            if(document.getElementById('volSlider')) document.getElementById('volSlider').value = volume;
        } else if(activeProcess === 'volume_tetap') {
            volume = newVol;
            temperature = newTemp;
            pressure = calculatePressureFromTempAndVolume(temperature, volume);
            if(document.getElementById('pressSlider')) document.getElementById('pressSlider').value = pressure;
        } else if(activeProcess === 'suhu_tetap') {
            temperature = newTemp;
            volume = newVol;
            pressure = (refPressure * refVolume) / volume;
            pressure = Math.min(MAX_PRESS, Math.max(MIN_PRESS, pressure));
            if(document.getElementById('pressSlider')) document.getElementById('pressSlider').value = pressure;
        }
        
        pressure = Math.min(MAX_PRESS, Math.max(MIN_PRESS, pressure));
        volume = Math.min(MAX_VOL, Math.max(MIN_VOL, volume));
        temperature = Math.min(MAX_TEMP, Math.max(MIN_TEMP, temperature));
        
        if(document.getElementById('pressValue')) document.getElementById('pressValue').innerText = pressure.toFixed(2);
        if(document.getElementById('volValue')) document.getElementById('volValue').innerText = volume.toFixed(2);
        if(document.getElementById('tempValue')) document.getElementById('tempValue').innerText = Math.round(temperature);
        if(document.getElementById('pressSlider')) document.getElementById('pressSlider').value = pressure;
        if(document.getElementById('volSlider')) document.getElementById('volSlider').value = volume;
        if(document.getElementById('tempSlider')) document.getElementById('tempSlider').value = temperature;
        
        let minW = 280, maxW = 900;
        let newW = minW + ((volume - MIN_VOL) / (MAX_VOL - MIN_VOL)) * (maxW - minW);
        containerWidth = Math.min(maxW, Math.max(minW, newW));
        if(canvas) { canvas.width = containerWidth; canvas.height = containerHeight; canvas.style.width = `${containerWidth}px`; }
        initParticlesSim();
        updateMolecularInfo();
    }
    
    class Particle {
        constructor(x, y, vx, vy, rad) { this.x = x; this.y = y; this.vx = vx; this.vy = vy; this.radius = rad; }
        draw(ctx, warna) {
            ctx.beginPath(); ctx.arc(this.x, this.y, this.radius, 0, Math.PI*2);
            ctx.fillStyle = warna; ctx.fill();
            // efek highlight
            ctx.beginPath(); ctx.arc(this.x-1.5, this.y-1.5, this.radius-3, 0, Math.PI*2);
            ctx.fillStyle = 'rgba(255,255,200,0.5)'; ctx.fill();
        }
        update(width, height) {
            this.x += this.vx; this.y += this.vy;
            let margin = 10, r = this.radius;
            if(this.x - r <= margin) { this.x = margin + r; this.vx = -this.vx; }
            if(this.x + r >= width - margin) { this.x = width - margin - r; this.vx = -this.vx; }
            if(this.y - r <= margin) { this.y = margin + r; this.vy = -this.vy; }
            if(this.y + r >= height - margin) { this.y = height - margin - r; this.vy = -this.vy; }
        }
    }
    
    function initParticlesSim() {
        particles = [];
        let radBase = (partikelJenis === 'monoatomik') ? 5 : 4.5;
        let sf = Math.sqrt(Math.max(0.01, temperature) / 400);
        // faktor kecepatan berdasarkan unsur
        let unsurFaktor = 1.0;
        if (partikelJenis === 'monoatomik') {
            const unsur = unsurMonoatomik.find(u => u.kode === unsurTerpilih) || unsurMonoatomik[0];
            unsurFaktor = unsur.kecepatanFaktor;
        } else {
            const unsur = unsurDiatomik.find(u => u.kode === unsurTerpilih) || unsurDiatomik[0];
            unsurFaktor = unsur.kecepatanFaktor;
        }
        for(let i=0; i<PARTICLE_COUNT; i++) {
            let margin = 16;
            let x = Math.random() * (containerWidth - 2*radBase - margin) + radBase + margin/2;
            let y = Math.random() * (containerHeight - 2*radBase - margin) + radBase + margin/2;
            let vx = (Math.random() - 0.5) * 3 * sf * unsurFaktor;
            let vy = (Math.random() - 0.5) * 3 * sf * unsurFaktor;
            particles.push(new Particle(x, y, vx, vy, radBase));
        }
    }
    
    function drawCanvas() {
        if(!ctx) return;
        ctx.clearRect(0, 0, containerWidth, containerHeight);
        ctx.fillStyle = '#0f172f'; ctx.fillRect(0, 0, containerWidth, containerHeight);
        ctx.strokeStyle = '#facc15'; ctx.lineWidth = 2; ctx.strokeRect(6, 6, containerWidth-12, containerHeight-12);
        ctx.strokeStyle = '#3b82f6'; ctx.setLineDash([8, 10]); ctx.strokeRect(12, 12, containerWidth-24, containerHeight-24); ctx.setLineDash([]);
        for(let p of particles) p.draw(ctx, currentWarna);
    }
    
    function animateSim() { if(!ctx) return; for(let p of particles) p.update(containerWidth, containerHeight); drawCanvas(); animationId = requestAnimationFrame(animateSim); }
    
    function setProcess(process) {
        activeProcess = process;
        
        const volSlider = document.getElementById('volSlider');
        const tempSlider = document.getElementById('tempSlider');
        const pressSlider = document.getElementById('pressSlider');
        
        if(process === 'volume_tetap') {
            if(volSlider) volSlider.disabled = true;
            if(tempSlider) tempSlider.disabled = false;
            if(pressSlider) pressSlider.disabled = false;
        } else if(process === 'suhu_tetap') {
            if(tempSlider) tempSlider.disabled = true;
            if(volSlider) volSlider.disabled = false;
            if(pressSlider) pressSlider.disabled = false;
        } else {
            if(pressSlider) pressSlider.disabled = true;
            if(volSlider) volSlider.disabled = false;
            if(tempSlider) tempSlider.disabled = false;
        }
        
        if(process === 'suhu_tetap') { refPressure = pressure; refVolume = volume; }
        updateByProcess();
        
        document.querySelectorAll('.btn-proses').forEach(btn => btn.classList.remove('aktif'));
        if(process === 'volume_tetap' && document.getElementById('btnVolumeTetap')) 
            document.getElementById('btnVolumeTetap').classList.add('aktif');
        else if(process === 'tekanan_tetap' && document.getElementById('btnTekananTetap')) 
            document.getElementById('btnTekananTetap').classList.add('aktif');
        else if(process === 'suhu_tetap' && document.getElementById('btnSuhuTetap')) 
            document.getElementById('btnSuhuTetap').classList.add('aktif');
    }
    
    function addParticles() { if(PARTICLE_COUNT < MAX_PARTICLES) { PARTICLE_COUNT += 5; if(document.getElementById('particleCounter')) document.getElementById('particleCounter').innerText = PARTICLE_COUNT; initParticlesSim(); updateMolecularInfo(); } }
    function removeParticles() { if(PARTICLE_COUNT > MIN_PARTICLES) { PARTICLE_COUNT -= 5; if(document.getElementById('particleCounter')) document.getElementById('particleCounter').innerText = PARTICLE_COUNT; initParticlesSim(); updateMolecularInfo(); } }
    
    function changePartikelJenis(jenis) {
        partikelJenis = jenis;
        if (jenis === 'monoatomik') {
            unsurTerpilih = 'He';
        } else {
            unsurTerpilih = 'O₂';
        }
        updateMassaDanWarna();
        initParticlesSim();
        updateMolecularInfo();
        
        // Update tampilan radio button dan dropdown
        const radioMono = document.getElementById('radioMono');
        const radioDi = document.getElementById('radioDi');
        if(radioMono) radioMono.checked = (jenis === 'monoatomik');
        if(radioDi) radioDi.checked = (jenis === 'diatomik');
        
        const unsurSelect = document.getElementById('unsurSelect');
        if(unsurSelect) {
            if (jenis === 'monoatomik') {
                unsurSelect.innerHTML = unsurMonoatomik.map(u => `<option value="${u.kode}" ${u.kode === unsurTerpilih ? 'selected' : ''}>${u.nama}</option>`).join('');
            } else {
                unsurSelect.innerHTML = unsurDiatomik.map(u => `<option value="${u.kode}" ${u.kode === unsurTerpilih ? 'selected' : ''}>${u.nama}</option>`).join('');
            }
        }
    }
    
    function changeUnsur(kode) {
        unsurTerpilih = kode;
        updateMassaDanWarna();
        initParticlesSim();
        updateMolecularInfo();
    }
    
    // ==================== RENDER ====================
    function render() {
        const root = document.getElementById("appRoot");
        if(!root) return;
        let html = `<div class="content-wrap">`;
        
        if(currentPage === "start") {
            html += `<div style="display:flex;flex-direction:column;align-items:center;justify-content:center;min-height:65vh;text-align:center;">
                <div id="starButton" style="background:radial-gradient(circle at 30% 25%, #ffdf8c, #e6a017);padding:2rem 3rem;border-radius:6rem;cursor:pointer;">
                    <i class="fas fa-atom" style="font-size:3rem;color:#fff3bf;"></i><br><span style="font-size:1rem;font-weight:600;color:#fff;">TERMOLAB</span>
                </div>
                <p style="margin-top:20px;color:#cbd5e6;">✦ Simulasi Gas Ideal | LKPD 3 Proses Termodinamika ✦</p>
            </div>`;
        }
        else if(currentPage === "petunjuk") {
            html += `<div><h2>📖 Panduan Simulasi</h2>
                <p style="margin:15px 0;color:#cbd5e6;">3 proses termodinamika dengan LKPD terpisah:</p>
                <ul style="color:#cbd5e6;margin-left:1.5rem;line-height:1.8;">
                    <li>🟢 <strong style="color:#34d399;">Volume Tetap</strong> - LKPD 1: Hukum Charles (P ∝ T)</li>
                    <li>🔵 <strong style="color:#60a5fa;">Tekanan Tetap</strong> - LKPD 2: Hukum Gay-Lussac (V ∝ T)</li>
                    <li>🟠 <strong style="color:#fbbf24;">Suhu Tetap</strong> - LKPD 3: Hukum Boyle (P ∝ 1/V)</li>
                    <li>➕➖ Tombol partikel: ubah jumlah molekul (5-200)</li>
                    <li>⚛️ Pilih jenis partikel (Monoatomik/Diatomik) dan unsur gas</li>
                </ul>
                <div style="display:flex;gap:1rem;justify-content:center;margin-top:1.5rem;flex-wrap:wrap;">
                    <button class="btn-primary" id="goSimulasi"><i class="fas fa-play"></i> Simulasi</button>
                    <button class="btn-primary" id="goMateri"><i class="fas fa-book"></i> Materi</button>
                    <button class="btn-primary" id="goLkpd"><i class="fas fa-database"></i> LKPD</button>
                </div>
            </div>`;
        }
        else if(currentPage === "materi") {
            html += `<div class="nav-buttons"><h2>📚 Materi Termodinamika</h2><button id="backHomeMateri" class="btn">🏠 Beranda</button></div>
            <div style="background:rgba(0,0,0,0.6);border-radius:1.5rem;padding:1.5rem;">
                <h3 style="color:#10b981;">🟢 Volume Tetap (Hukum Charles)</h3><p style="color:#cbd5e6;">P₁/T₁ = P₂/T₂ | Tekanan ∝ Suhu</p>
                <h3 style="color:#3b82f6;">🔵 Tekanan Tetap (Hukum Gay-Lussac)</h3><p style="color:#cbd5e6;">V₁/T₁ = V₂/T₂ | Volume ∝ Suhu</p>
                <h3 style="color:#f59e0b;">🟠 Suhu Tetap (Hukum Boyle)</h3><p style="color:#cbd5e6;">P₁V₁ = P₂V₂ | Tekanan ∝ 1/Volume</p>
            </div>
            <button id="backMateriPetunjuk" class="btn" style="margin-top:1rem;">← Kembali</button>`;
        }
        else if(currentPage === "lkpd_menu") {
            html += `<div class="nav-buttons"><h2><i class="fas fa-pen-fancy"></i> Pilih Lembar Kerja Peserta Didik (LKPD)</h2><button id="backHomeLkpdMenu" class="btn">🏠 Beranda</button></div>
            <div style="display:flex; gap:1rem; justify-content:center; flex-wrap:wrap; margin:1.5rem 0;">
                <div class="lkpd-menu-card volume-tetap" id="lkpdVolumeTetap"><i class="fas fa-arrows-alt-v"></i><h3>LKPD 1</h3><div class="judul">Volume Tetap</div><div class="sub">Hubungan P dengan T</div></div>
                <div class="lkpd-menu-card tekanan-tetap" id="lkpdTekananTetap"><i class="fas fa-chart-line"></i><h3>LKPD 2</h3><div class="judul">Tekanan Tetap</div><div class="sub">Hubungan V dengan T</div></div>
                <div class="lkpd-menu-card suhu-tetap" id="lkpdSuhuTetap"><i class="fas fa-thermometer-half"></i><h3>LKPD 3</h3><div class="judul">Suhu Tetap</div><div class="sub">Hubungan P dengan V</div></div>
            </div>
            <div class="feedback-guru"><i class="fas fa-info-circle"></i><span>Pilih LKPD sesuai proses yang ingin dipelajari</span></div>`;
        }
        else if(currentPage === "lkpd_form") {
            const data = lkpdData[lkpdMode];
            let containerClass = "", badgeClass = "", badgeText = "", titleIcon = "", tujuanText = "";
            let col1 = "", col2 = "", col3 = "";
            let fixedVal = "";
            
            if(lkpdMode === 'volume_tetap') { 
                containerClass = "volume-tetap"; badgeClass = "badge-volume"; badgeText = "Volume Tetap | Hukum Charles"; titleIcon = "🟢"; 
                tujuanText = "Menyelidiki hubungan antara <strong>Tekanan (P)</strong> dengan <strong>Suhu (T)</strong> pada Volume tetap (V = 3,00 L)";
                col1 = "Tekanan (atm)"; col2 = "Suhu (K)"; col3 = "Volume (L) <span class='tetap-badge'>tetap 3,00</span>";
                fixedVal = "3.00";
            } else if(lkpdMode === 'tekanan_tetap') { 
                containerClass = "tekanan-tetap"; badgeClass = "badge-tekanan"; badgeText = "Tekanan Tetap | Hukum Gay-Lussac"; titleIcon = "🔵"; 
                tujuanText = "Menyelidiki hubungan antara <strong>Volume (V)</strong> dengan <strong>Suhu (T)</strong> pada Tekanan tetap (P = 2,50 atm)";
                col1 = "Volume (L)"; col2 = "Suhu (K)"; col3 = "Tekanan (atm) <span class='tetap-badge'>tetap 2,50</span>";
                fixedVal = "2.50";
            } else { 
                containerClass = "suhu-tetap"; badgeClass = "badge-suhu"; badgeText = "Suhu Tetap | Hukum Boyle"; titleIcon = "🟠"; 
                tujuanText = "Menyelidiki hubungan antara <strong>Tekanan (P)</strong> dengan <strong>Volume (V)</strong> pada Suhu tetap (T = 400 K)";
                col1 = "Tekanan (atm)"; col2 = "Volume (L)"; col3 = "Suhu (K) <span class='tetap-badge'>tetap 400</span>";
                fixedVal = "400";
            }
            
            let tabelRows = "";
            for(let i = 0; i < 5; i++) {
                let rowData = (data.tabel && data.tabel[i]) ? data.tabel[i] : {};
                let val1 = escapeHtml(rowData.val1 || "");
                let val2 = escapeHtml(rowData.val2 || "");
                tabelRows += `<tr>
                    <td><input type="text" class="tabel-col1" data-row="${i}" placeholder="Percobaan ${i+1}" value="${val1}"></td>
                    <td><input type="text" class="tabel-col2" data-row="${i}" placeholder="Percobaan ${i+1}" value="${val2}"></td>
                    <td><input type="text" class="tabel-col3" data-row="${i}" value="${fixedVal}" readonly style="color:#94a3b8; text-align:center;"></td>
                </tr>`;
            }
            
            html += `<div class="nav-buttons">
                <button id="backToLkpdMenu" class="btn"><i class="fas fa-arrow-left"></i> Pilihan LKPD</button>
                <button id="backHomeLkpdForm" class="btn"><i class="fas fa-home"></i> Beranda</button>
            </div>
            <div class="lkpd-form-container ${containerClass}">
                <div class="lkpd-header"><h2>${titleIcon} ${lkpdMode === 'volume_tetap' ? 'LKPD 1' : (lkpdMode === 'tekanan_tetap' ? 'LKPD 2' : 'LKPD 3')}</h2><span class="badge ${badgeClass}">${badgeText}</span></div>
                
                <div class="lkpd-section"><div class="lkpd-section-title"><i class="fas fa-bullseye"></i> Tujuan Percobaan</div><div class="tujuan-text">${tujuanText}</div></div>
                
                <div class="lkpd-section"><div class="lkpd-section-title"><i class="fas fa-question-circle"></i> Rumusan Masalah</div><textarea class="lkpd-textarea" id="lkpdRumusan" rows="2" placeholder="Tuliskan rumusan masalah...">${escapeHtml(data.rumusan)}</textarea></div>
                
                <div class="lkpd-section"><div class="lkpd-section-title"><i class="fas fa-lightbulb"></i> Hipotesis</div><textarea class="lkpd-textarea" id="lkpdHipotesis" rows="2" placeholder="Tuliskan hipotesis...">${escapeHtml(data.hipotesis)}</textarea></div>
                
                <div class="lkpd-section"><div class="lkpd-section-title"><i class="fas fa-table"></i> Tabel Pengamatan (5 Kali Percobaan)</div>
                    <table class="observ-table">
                        <thead><tr><th>${col1}</th><th>${col2}</th><th>${col3}</th></tr></thead>
                        <tbody>${tabelRows}</tbody>
                    </table>
                    <div class="info-text"><i class="fas fa-info-circle"></i> Lakukan 5 kali percobaan dengan mengubah nilai pada simulasi, lalu catat hasilnya.</div>
                </div>
                
                <div class="lkpd-section"><div class="lkpd-section-title"><i class="fas fa-chart-line"></i> Analisis Data</div><textarea class="lkpd-textarea" id="lkpdAnalisis" rows="3" placeholder="Analisis hubungan antar variabel...">${escapeHtml(data.analisis)}</textarea></div>
                
                <div class="lkpd-section"><div class="lkpd-section-title"><i class="fas fa-check-double"></i> Kesimpulan</div><textarea class="lkpd-textarea" id="lkpdKesimpulan" rows="2" placeholder="Tuliskan kesimpulan...">${escapeHtml(data.kesimpulan)}</textarea></div>
                
                <button id="saveLkpdBtn" class="save-btn"><i class="fas fa-save"></i> Simpan LKPD</button>
                <div class="feedback-guru"><i class="fas fa-chalkboard-user"></i><span>Data tersimpan otomatis di browser</span></div>
            </div>`;
        }
        else if(currentPage === "simulasi") {
            let pressDisabled = (activeProcess === 'tekanan_tetap') ? 'disabled' : '';
            let volDisabled = (activeProcess === 'volume_tetap') ? 'disabled' : '';
            let tempDisabled = (activeProcess === 'suhu_tetap') ? 'disabled' : '';
            
            // Opsi unsur untuk dropdown
            let unsurOptions = '';
            if (partikelJenis === 'monoatomik') {
                unsurOptions = unsurMonoatomik.map(u => `<option value="${u.kode}" ${u.kode === unsurTerpilih ? 'selected' : ''}>${u.nama}</option>`).join('');
            } else {
                unsurOptions = unsurDiatomik.map(u => `<option value="${u.kode}" ${u.kode === unsurTerpilih ? 'selected' : ''}>${u.nama}</option>`).join('');
            }
            
            html += `<div class="nav-buttons"><h2><i class="fas fa-microscope"></i> Simulasi Gas</h2><button id="backHomeSim" class="btn">🏠 Beranda</button></div>
            <div class="sim-card">
                <div class="proses-buttons">
                    <button id="btnVolumeTetap" class="btn-proses ${activeProcess === 'volume_tetap' ? 'aktif' : ''}"><i class="fas fa-arrows-alt-v"></i> Volume Tetap</button>
                    <button id="btnTekananTetap" class="btn-proses ${activeProcess === 'tekanan_tetap' ? 'aktif' : ''}"><i class="fas fa-chart-line"></i> Tekanan Tetap</button>
                    <button id="btnSuhuTetap" class="btn-proses ${activeProcess === 'suhu_tetap' ? 'aktif' : ''}"><i class="fas fa-thermometer-half"></i> Suhu Tetap</button>
                </div>
                <div class="canvas-orb"><canvas id="gasCanvas" width="800" height="400"></canvas></div>
                <div style="display:flex;flex-wrap:wrap;gap:1rem;">
                    <div class="param-pod"><label><i class="fas fa-tachometer-alt"></i> Tekanan (atm)</label><input type="range" id="pressSlider" min="0" max="5" step="0.02" value="${pressure}" ${pressDisabled}><span id="pressValue" class="value-badge">${pressure.toFixed(2)} atm</span></div>
                    <div class="param-pod"><label><i class="fas fa-expand-alt"></i> Volume (L)</label><input type="range" id="volSlider" min="0.5" max="10" step="0.05" value="${volume}" ${volDisabled}><span id="volValue" class="value-badge">${volume.toFixed(2)} L</span></div>
                    <div class="param-pod"><label><i class="fas fa-thermometer-half"></i> Suhu (K)</label><input type="range" id="tempSlider" min="0" max="800" step="5" value="${temperature}" ${tempDisabled}><span id="tempValue" class="value-badge">${Math.round(temperature)} K</span></div>
                </div>
                
                <!-- PEMILIH JENIS PARTIKEL DAN UNSUR -->
                <div class="partikel-selector">
                    <label><i class="fas fa-atom"></i> Jenis Partikel:</label>
                    <div class="partikel-option ${partikelJenis === 'monoatomik' ? 'active' : ''}" onclick="window.changePartikelJenis('monoatomik')">
                        <input type="radio" name="jenisPartikel" id="radioMono" ${partikelJenis === 'monoatomik' ? 'checked' : ''}> Monoatomik
                    </div>
                    <div class="partikel-option ${partikelJenis === 'diatomik' ? 'active' : ''}" onclick="window.changePartikelJenis('diatomik')">
                        <input type="radio" name="jenisPartikel" id="radioDi" ${partikelJenis === 'diatomik' ? 'checked' : ''}> Diatomik
                    </div>
                    <select id="unsurSelect" class="unsur-select" onchange="window.changeUnsur(this.value)">
                        ${unsurOptions}
                    </select>
                </div>
                
                <div class="molecular-info">
                    <div class="info-card"><i class="fas fa-bolt"></i><div class="info-label">ENERGI KINETIK</div><div class="info-value" id="kineticEnergy">${calculateAvgKineticEnergy()}</div><div class="info-unit">× 10⁻¹⁹ Joule</div></div>
                    <div class="info-card"><i class="fas fa-tachometer-alt"></i><div class="info-label">KECEPATAN RMS</div><div class="info-value" id="avgSpeed">${calculateRMSSpeed()}</div><div class="info-unit">meter/detik</div></div>
                    <div class="info-card"><i class="fas fa-hand-fist"></i><div class="info-label">TUMBUKAN / DETIK</div><div class="info-value" id="collisionRate">${calculateCollisionsPerSecond()}</div><div class="info-unit">kali per detik</div></div>
                </div>
                
                <div class="particle-controller">
                    <span><i class="fas fa-chart-simple"></i> JUMLAH PARTIKEL</span>
                    <div class="counter-group"><button id="btnKurangiPartikel"><i class="fas fa-minus"></i></button><span id="particleCounter" class="particle-num">${PARTICLE_COUNT}</span><button id="btnTambahPartikel"><i class="fas fa-plus"></i></button></div>
                    <span style="font-size:0.7rem; color:#94a3b8;">Min 5 - Maks 200 partikel</span>
                </div>
            </div>
            <button id="backToPetunjukSim" class="btn" style="margin-top:0.5rem;">← Kembali ke Petunjuk</button>`;
        }
        
        html += `</div>`;
        root.innerHTML = html;
        
        // ========== EVENT HANDLERS ==========
        
        if(currentPage === "start") {
            const starBtn = document.getElementById("starButton");
            if(starBtn) starBtn.addEventListener("click",()=>{ currentPage="petunjuk"; render(); });
        }
        
        if(currentPage === "petunjuk") {
            const goSim = document.getElementById("goSimulasi");
            const goMat = document.getElementById("goMateri");
            const goLkpd = document.getElementById("goLkpd");
            if(goSim) goSim.addEventListener("click",()=>{ currentPage="simulasi"; render(); });
            if(goMat) goMat.addEventListener("click",()=>{ currentPage="materi"; render(); });
            if(goLkpd) goLkpd.addEventListener("click",()=>{ currentPage="lkpd_menu"; render(); });
        }
        
        if(currentPage === "materi") {
            const backHome = document.getElementById("backHomeMateri");
            const backPet = document.getElementById("backMateriPetunjuk");
            if(backHome) backHome.addEventListener("click",()=>{ currentPage="petunjuk"; render(); });
            if(backPet) backPet.addEventListener("click",()=>{ currentPage="petunjuk"; render(); });
        }
        
        if(currentPage === "lkpd_menu") {
            const backHome = document.getElementById("backHomeLkpdMenu");
            const volBtn = document.getElementById("lkpdVolumeTetap");
            const tekBtn = document.getElementById("lkpdTekananTetap");
            const suhuBtn = document.getElementById("lkpdSuhuTetap");
            
            if(backHome) backHome.addEventListener("click",()=>{ currentPage="petunjuk"; render(); });
            if(volBtn) volBtn.addEventListener("click",()=>{ lkpdMode = "volume_tetap"; currentPage="lkpd_form"; render(); });
            if(tekBtn) tekBtn.addEventListener("click",()=>{ lkpdMode = "tekanan_tetap"; currentPage="lkpd_form"; render(); });
            if(suhuBtn) suhuBtn.addEventListener("click",()=>{ lkpdMode = "suhu_tetap"; currentPage="lkpd_form"; render(); });
        }
        
        if(currentPage === "lkpd_form") {
            const backMenu = document.getElementById("backToLkpdMenu");
            const backHome = document.getElementById("backHomeLkpdForm");
            const saveBtn = document.getElementById("saveLkpdBtn");
            
            if(backMenu) backMenu.addEventListener("click",()=>{ currentPage="lkpd_menu"; render(); });
            if(backHome) backHome.addEventListener("click",()=>{ currentPage="petunjuk"; render(); });
            if(saveBtn) saveBtn.addEventListener("click",()=>{
                let tabelData = [];
                let col1Inputs = document.querySelectorAll('.tabel-col1');
                let col2Inputs = document.querySelectorAll('.tabel-col2');
                for(let i = 0; i < 5; i++) {
                    tabelData.push({
                        val1: col1Inputs[i]?.value || "",
                        val2: col2Inputs[i]?.value || ""
                    });
                }
                lkpdData[lkpdMode] = {
                    tujuan: lkpdData[lkpdMode]?.tujuan || "",
                    rumusan: document.getElementById("lkpdRumusan")?.value||"",
                    hipotesis: document.getElementById("lkpdHipotesis")?.value||"",
                    tabel: tabelData,
                    analisis: document.getElementById("lkpdAnalisis")?.value||"",
                    kesimpulan: document.getElementById("lkpdKesimpulan")?.value||""
                };
                saveLKPDLocal();
                alert("✓ LKPD berhasil disimpan!");
            });
        }
        
        if(currentPage === "simulasi") {
            canvas = document.getElementById("gasCanvas");
            if(canvas) {
                ctx = canvas.getContext("2d");
                containerWidth = 800; containerHeight = 400;
                canvas.width = 800; canvas.height = 400;
                initParticlesSim();
                if(animationId) cancelAnimationFrame(animationId);
                animateSim();
                
                const pressSlider = document.getElementById("pressSlider");
                const volSlider = document.getElementById("volSlider");
                const tempSlider = document.getElementById("tempSlider");
                const btnTambah = document.getElementById("btnTambahPartikel");
                const btnKurangi = document.getElementById("btnKurangiPartikel");
                const btnVolTetap = document.getElementById("btnVolumeTetap");
                const btnTekTetap = document.getElementById("btnTekananTetap");
                const btnSuhuTetap = document.getElementById("btnSuhuTetap");
                const backHome = document.getElementById("backHomeSim");
                const backPet = document.getElementById("backToPetunjukSim");
                
                if(pressSlider) pressSlider.addEventListener("input", (e) => { pressure = parseFloat(e.target.value); updateByProcess(); });
                if(volSlider) volSlider.addEventListener("input", (e) => { volume = parseFloat(e.target.value); updateByProcess(); });
                if(tempSlider) tempSlider.addEventListener("input", (e) => { temperature = parseFloat(e.target.value); updateByProcess(); });
                if(btnTambah) btnTambah.addEventListener("click", addParticles);
                if(btnKurangi) btnKurangi.addEventListener("click", removeParticles);
                if(btnVolTetap) btnVolTetap.addEventListener("click", () => setProcess("volume_tetap"));
                if(btnTekTetap) btnTekTetap.addEventListener("click", () => setProcess("tekanan_tetap"));
                if(btnSuhuTetap) btnSuhuTetap.addEventListener("click", () => setProcess("suhu_tetap"));
                if(backHome) backHome.addEventListener("click",()=>{ currentPage="petunjuk"; render(); });
                if(backPet) backPet.addEventListener("click",()=>{ currentPage="petunjuk"; render(); });
                
                // Expose fungsi untuk global agar bisa dipanggil dari onclick
                window.changePartikelJenis = changePartikelJenis;
                window.changeUnsur = changeUnsur;
                
                updateByProcess();
            }
        }
    }
    render();
</script>
</body>
</html>
