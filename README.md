<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Animasi Gerak Partikel - Teori Kinetik Gas (SMA)</title>
    <style>
        * {
            box-sizing: border-box;
        }
        body {
            background: linear-gradient(145deg, #1b2b4e 0%, #152238 100%);
            font-family: 'Segoe UI', Roboto, system-ui, sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            margin: 0;
            padding: 15px;
        }
        .card {
            background: rgba(255,255,255,0.1);
            backdrop-filter: blur(10px);
            border-radius: 2.5rem;
            padding: 25px 25px 30px 25px;
            box-shadow: 0 25px 40px rgba(0,0,0,0.6), 0 0 0 2px rgba(255,255,240,0.15);
            max-width: 1200px;
            width: 100%;
        }
        h1 {
            text-align: center;
            margin: 0 0 12px 0;
            font-weight: 500;
            font-size: 2.2rem;
            letter-spacing: 1px;
            color: #f0f9ff;
            text-shadow: 0 4px 8px #00000040;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 10px;
        }
        h1 span {
            background: #ffd966cc;
            padding: 0 16px;
            border-radius: 60px;
            font-size: 1.2rem;
            font-weight: 600;
            color: #1e2b41;
            text-shadow: none;
            box-shadow: inset 0 -2px 0 #a37c2e;
        }
        .canvas-container {
            background: #0b1421;
            border-radius: 32px;
            padding: 10px;
            box-shadow: inset 0 -3px 8px #00000099, 0 12px 25px black;
            border: 1px solid #6a7ca0;
        }
        canvas {
            display: block;
            width: 100%;
            height: auto;
            background: radial-gradient(circle at 30% 30%, #25344f, #101624);
            border-radius: 24px;
            cursor: pointer;
            border: 2px solid #5f73a1;
            transition: all 0.2s;
        }
        canvas:active {
            border-color: #ffbf4b;
        }
        .info-panel {
            display: flex;
            flex-wrap: wrap;
            gap: 25px;
            margin-top: 25px;
            color: #eef5ff;
        }
        .petunjuk {
            flex: 2;
            min-width: 280px;
            background: #1f2b41dd;
            border-radius: 28px;
            padding: 20px 25px;
            border: 1px solid #8096c0;
            backdrop-filter: blur(4px);
            box-shadow: 0 10px 18px #00000055;
        }
        .petunjuk h3 {
            display: flex;
            align-items: center;
            gap: 12px;
            font-weight: 400;
            font-size: 1.8rem;
            margin: 0 0 10px 0;
            color: #ffdb9d;
            border-bottom: 2px dashed #9ab0dd;
            padding-bottom: 8px;
        }
        .steps {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 18px;
        }
        .step-item {
            background: #0f1a2c;
            padding: 16px 18px;
            border-radius: 24px;
            border-left: 6px solid #ffb347;
            box-shadow: 0 6px 0 #04080f;
        }
        .step-item strong {
            display: block;
            font-size: 1.2rem;
            color: #ffd966;
            margin-bottom: 8px;
        }
        .step-item p {
            margin: 5px 0 0 0;
            font-size: 0.95rem;
            line-height: 1.5;
            color: #cddfff;
        }
        .keterangan {
            flex: 1.3;
            min-width: 280px;
            background: #1b273fdd;
            border-radius: 28px;
            padding: 20px 25px;
            border: 1px solid #ffc96e;
            backdrop-filter: blur(4px);
        }
        .keterangan h3 {
            font-size: 1.7rem;
            font-weight: 400;
            color: #ffe1aa;
            margin: 0 0 20px 0;
            display: flex;
            align-items: center;
            gap: 8px;
        }
        .stat-list {
            display: flex;
            flex-direction: column;
            gap: 18px;
        }
        .stat-item {
            display: flex;
            align-items: center;
            gap: 14px;
            background: #0b1322b3;
            border-radius: 40px;
            padding: 10px 18px;
            border: 1px solid #3d506e;
        }
        .stat-icon {
            font-size: 2rem;
        }
        .stat-desc {
            font-size: 1.1rem;
        }
        .stat-desc .value {
            font-weight: 700;
            font-size: 1.7rem;
            color: #ffb347;
            line-height: 1.2;
        }
        
        /* Pengaturan suhu Kelvin */
        .kelvin-control {
            background: #0f1a2c;
            border-radius: 40px;
            padding: 18px 20px;
            margin: 15px 0 10px 0;
            border: 1px solid #ffb85b;
        }
        .kelvin-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 15px;
        }
        .kelvin-header span {
            font-size: 1.4rem;
            font-weight: bold;
            color: #ffdb9d;
        }
        .kelvin-value {
            background: #263a5c;
            padding: 5px 18px;
            border-radius: 40px;
            font-size: 2rem;
            font-weight: 700;
            color: #ffd966;
            border: 1px solid #fbbf24;
        }
        .slider-container {
            display: flex;
            align-items: center;
            gap: 15px;
        }
        .slider-label {
            min-width: 45px;
            color: #b0c6f0;
            font-weight: 500;
        }
        input[type=range] {
            flex: 1;
            height: 10px;
            background: linear-gradient(90deg, #22c1c3, #3b82f6, #8b5cf6, #ef4444);
            border-radius: 20px;
            -webkit-appearance: none;
            appearance: none;
        }
        input[type=range]::-webkit-slider-thumb {
            -webkit-appearance: none;
            width: 25px;
            height: 25px;
            background: white;
            border-radius: 50%;
            border: 3px solid #ffb347;
            box-shadow: 0 4px 8px black;
            cursor: pointer;
        }
        .thermometer-note {
            display: flex;
            justify-content: space-between;
            margin-top: 10px;
            color: #9ab0dd;
            font-size: 0.9rem;
        }
        .zero-kelvin-note {
            background: #3b1e4b;
            border-radius: 30px;
            padding: 8px 15px;
            margin-top: 12px;
            text-align: center;
            font-size: 0.95rem;
            color: #e2b3ff;
            border: 1px dashed #c084fc;
        }
        
        .tombol-interaktif {
            display: flex;
            flex-wrap: wrap;
            justify-content: center;
            gap: 12px;
            margin: 15px 0 5px 0;
        }
        button {
            background: #263a5c;
            border: none;
            color: white;
            font-size: 1.1rem;
            font-weight: 500;
            padding: 12px 20px;
            border-radius: 60px;
            display: inline-flex;
            align-items: center;
            gap: 8px;
            box-shadow: 0 8px 0 #0e1628, 0 6px 12px black;
            transition: 0.08s linear;
            cursor: pointer;
            border: 1px solid #6f8bc0;
            flex: 1 0 auto;
            justify-content: center;
        }
        button:active {
            transform: translateY(6px);
            box-shadow: 0 2px 0 #0e1628, 0 6px 12px black;
        }
        .reset-btn {
            background: #3f3152;
            border-color: #c896ff;
        }
        .footer-note {
            text-align: center;
            margin-top: 18px;
            font-size: 0.95rem;
            color: #b3ceff;
        }
    </style>
</head>
<body>
<div class="card">
    <h1>
        ⚛️ GERAK PARTIKEL GAS IDEAL 
        <span>SMA</span>
    </h1>

    <!-- Canvas untuk animasi -->
    <div class="canvas-container">
        <canvas id="gasCanvas" width="1000" height="500"></canvas>
    </div>

    <!-- Tombol kendali cepat -->
    <div class="tombol-interaktif">
        <button id="tambahPartikelBtn">➕ Tambah partikel</button>
        <button id="kurangPartikelBtn">➖ Kurangi partikel</button>
        <button id="resetBtn" class="reset-btn">🔄 Reset awal (300 K)</button>
    </div>

    <!-- Panel informasi & petunjuk penggunaan -->
    <div class="info-panel">
        <div class="petunjuk">
            <h3>
                <span>📘</span> PETUNJUK PENGGUNAAN
            </h3>
            <div class="steps">
                <div class="step-item">
                    <strong>🖱️ Klik & drag di kotak</strong>
                    <p>Tekan dan geser mouse: partikel yang terkena akan terdorong mengikuti arah geseran.</p>
                </div>
                <div class="step-item">
                    <strong>➕ Tombol Tambah</strong>
                    <p>Menambah 3 partikel baru. Gerakannya menyesuaikan suhu Kelvin saat ini.</p>
                </div>
                <div class="step-item">
                    <strong>➖ Tombol Kurang</strong>
                    <p>Menghapus 3 partikel terakhir.</p>
                </div>
                <div class="step-item">
                    <strong>🌡️ Slider Suhu Kelvin (0K - 500K)</strong>
                    <p>Geser slider untuk mengatur suhu gas. Pada <b>0 Kelvin (nol mutlak)</b>, partikel berhenti total (energi kinetik nol).</p>
                </div>
                <div class="step-item">
                    <strong>🔄 Reset</strong>
                    <p>Kembali ke 20 partikel dengan suhu 300 Kelvin (suhu ruang).</p>
                </div>
            </div>
        </div>

        <div class="keterangan">
            <h3>📊 STATUS GAS (KELVIN)</h3>
            
            <!-- SUHU KELVIN DENGAN SLIDER (0K - 500K) -->
            <div class="kelvin-control">
                <div class="kelvin-header">
                    <span>🌡️ SUHU GAS</span>
                    <span class="kelvin-value" id="kelvinDisplay">300 K</span>
                </div>
                <div class="slider-container">
                    <span class="slider-label">0 K</span>
                    <input type="range" id="kelvinSlider" min="0" max="500" value="300" step="1">
                    <span class="slider-label">500 K</span>
                </div>
                <div class="thermometer-note">
                    <span>❄️ Nol Mutlak (0 K)</span>
                    <span>🔥 500 K</span>
                </div>
                <div class="zero-kelvin-note" id="zeroNote" style="display: none;">
                    ⚛️ 0 KELVIN - Partikel diam total (energi kinetik nol)
                </div>
            </div>
            
            <div class="stat-list">
                <div class="stat-item">
                    <span class="stat-icon">🔹</span>
                    <div class="stat-desc">
                        Jumlah partikel <br><span class="value" id="jumlahPartikelDisplay">20</span>
                    </div>
                </div>
                <div class="stat-item">
                    <span class="stat-icon">📏</span>
                    <div class="stat-desc">
                        Kecepatan rata-rata <br><span class="value" id="kecepatanDisplay">3.20</span> px/frame
                    </div>
                </div>
                <div class="stat-item">
                    <span class="stat-icon">⚡</span>
                    <div class="stat-desc">
                        Energi kinetik rata-rata <br><span class="value" id="ekDisplay">100%</span>
                    </div>
                </div>
            </div>
            
            <div class="footer-note">
                ⚡ Geser kursor untuk mendorong partikel | Pada 0 K partikel tidak bergerak
            </div>
        </div>
    </div>
</div>

<script>
(function() {
    const canvas = document.getElementById('gasCanvas');
    const ctx = canvas.getContext('2d');

    // Ukuran kanvas (sesuai atribut)
    const W = 1000;
    const H = 500;

    // ==========  PARAMETER GLOBAL ==========
    let particles = [];               // array partikel {x, y, vx, vy, r}
    const BASE_RADIUS = 7;            // radius tampak, tidak berubah massa

    // Referensi suhu: 300 K = kecepatan rata-rata sekitar 3.2 px/frame
    const REF_SPEED_AT_300K = 3.2;    // kecepatan referensi pada 300K
    
    // Suhu saat ini (Kelvin) - bisa 0 K
    let currentKelvin = 300;
    
    // Jumlah partikel target
    let targetCount = 20;

    // ==========  FUNGSI KECEPATAN BERDASARKAN SUHU ==========
    // Dalam teori kinetik gas: v_rms ~ √T, jadi kecepatan sebanding dengan akar suhu
    // Pada 0 K, kecepatan = 0
    function getSpeedFactorFromKelvin(kelvin) {
        if (kelvin <= 0) return 0; // Pada 0 K, faktor = 0 (diam total)
        // Pada 300 K, faktor = 1.0
        return Math.sqrt(kelvin / 300);
    }
    
    // Update kecepatan semua partikel berdasarkan suhu Kelvin
    function applyKelvinTemperature(newKelvin) {
        // Pastikan newKelvin tidak negatif
        newKelvin = Math.max(0, newKelvin);
        
        const oldFactor = getSpeedFactorFromKelvin(currentKelvin);
        const newFactor = getSpeedFactorFromKelvin(newKelvin);
        
        // Hindari pembagian dengan nol
        let ratio = 0;
        if (oldFactor > 0) {
            ratio = newFactor / oldFactor;
        } else {
            // Jika oldFactor = 0 (diam), kita perlu set kecepatan baru dari nol
            // Gunakan pendekatan khusus
            if (newFactor > 0) {
                // Beri kecepatan acak dengan besar sesuai newFactor
                const baseSpeedRef = REF_SPEED_AT_300K / Math.sqrt(300);
                particles.forEach(p => {
                    const angle = Math.random() * 2 * Math.PI;
                    const speed = baseSpeedRef * Math.sqrt(newKelvin);
                    p.vx = Math.cos(angle) * speed;
                    p.vy = Math.sin(angle) * speed;
                });
                currentKelvin = newKelvin;
                updateInfo();
                return;
            } else {
                ratio = 1; // sama-sama 0
            }
        }
        
        // Kalikan semua kecepatan dengan ratio
        particles.forEach(p => {
            p.vx *= ratio;
            p.vy *= ratio;
        });
        
        currentKelvin = newKelvin;
        updateInfo();
    }
    
    // Set suhu langsung (untuk inisialisasi)
    function setKelvinDirect(kelvin) {
        kelvin = Math.max(0, kelvin);
        const factor = getSpeedFactorFromKelvin(kelvin);
        
        // Hitung kecepatan target berdasarkan faktor
        // Kita ingin pada 300K, kecepatan rata-rata = REF_SPEED_AT_300K
        const baseSpeedRef = REF_SPEED_AT_300K / Math.sqrt(300);
        
        particles.forEach(p => {
            if (factor === 0) {
                // Diam total
                p.vx = 0;
                p.vy = 0;
            } else {
                // Pertahankan arah, ubah besar
                const speed = Math.hypot(p.vx, p.vy);
                if (speed > 0.001) {
                    const targetSpeed = baseSpeedRef * Math.sqrt(kelvin);
                    const scale = targetSpeed / speed;
                    p.vx *= scale;
                    p.vy *= scale;
                } else {
                    // Jika partikel diam, beri kecepatan acak
                    const angle = Math.random() * 2 * Math.PI;
                    const targetSpeed = baseSpeedRef * Math.sqrt(kelvin);
                    p.vx = Math.cos(angle) * targetSpeed;
                    p.vy = Math.sin(angle) * targetSpeed;
                }
            }
        });
        
        currentKelvin = kelvin;
        updateInfo();
    }

    // ==========  INISIALISASI ==========
    function initParticles(count, kelvin = 300) {
        const newParticles = [];
        const baseSpeedRef = REF_SPEED_AT_300K / Math.sqrt(300); // konstanta
        
        for (let i = 0; i < count; i++) {
            const x = Math.random() * (W - 2 * BASE_RADIUS) + BASE_RADIUS;
            const y = Math.random() * (H - 2 * BASE_RADIUS) + BASE_RADIUS;
            
            let vx, vy;
            
            if (kelvin === 0) {
                // Diam total
                vx = 0;
                vy = 0;
            } else {
                const angle = Math.random() * 2 * Math.PI;
                // Kecepatan berdasarkan suhu Kelvin
                const targetSpeed = baseSpeedRef * Math.sqrt(kelvin);
                // Variasi ±30% agar tidak seragam
                const speedVariation = 0.7 + Math.random() * 0.6; // 0.7 - 1.3
                const speed = targetSpeed * speedVariation;
                
                vx = Math.cos(angle) * speed;
                vy = Math.sin(angle) * speed;
            }

            newParticles.push({
                x, y, vx, vy,
                r: BASE_RADIUS
            });
        }
        return newParticles;
    }

    // Set partikel awal (300 K)
    particles = initParticles(targetCount, 300);
    currentKelvin = 300;

    // ==========  UPDATE JUMLAH PARTIKEL ==========
    function setParticleCount(newCount) {
        newCount = Math.min(70, Math.max(2, newCount));
        targetCount = newCount;

        const currentCount = particles.length;
        if (newCount > currentCount) {
            // tambah partikel baru dengan suhu saat ini
            const baseSpeedRef = REF_SPEED_AT_300K / Math.sqrt(300);
            
            for (let i = 0; i < newCount - currentCount; i++) {
                const x = Math.random() * (W - 2 * BASE_RADIUS) + BASE_RADIUS;
                const y = Math.random() * (H - 2 * BASE_RADIUS) + BASE_RADIUS;
                
                let vx, vy;
                
                if (currentKelvin === 0) {
                    vx = 0;
                    vy = 0;
                } else {
                    const angle = Math.random() * 2 * Math.PI;
                    const targetSpeed = baseSpeedRef * Math.sqrt(currentKelvin);
                    const speedVariation = 0.7 + Math.random() * 0.6;
                    const speed = targetSpeed * speedVariation;
                    vx = Math.cos(angle) * speed;
                    vy = Math.sin(angle) * speed;
                }

                particles.push({ x, y, vx, vy, r: BASE_RADIUS });
            }
        } else if (newCount < currentCount) {
            particles.splice(newCount, currentCount - newCount);
        }
        updateInfo();
    }

    // ==========  UPDATE INFORMASI ==========
    function updateInfo() {
        const jumlahSpan = document.getElementById('jumlahPartikelDisplay');
        const kecepatanSpan = document.getElementById('kecepatanDisplay');
        const ekSpan = document.getElementById('ekDisplay');
        const kelvinSpan = document.getElementById('kelvinDisplay');
        const kelvinSlider = document.getElementById('kelvinSlider');
        const zeroNote = document.getElementById('zeroNote');

        jumlahSpan.innerText = particles.length;

        // Hitung rata-rata kelajuan (speed)
        let totalSpeed = 0;
        if (particles.length > 0) {
            totalSpeed = particles.reduce((acc, p) => acc + Math.hypot(p.vx, p.vy), 0) / particles.length;
        } else {
            totalSpeed = 0;
        }
        
        kecepatanSpan.innerText = totalSpeed.toFixed(2);
        
        // Energi kinetik rata-rata (sebanding dengan kecepatan^2)
        // Tampilkan sebagai persentase relatif terhadap 300K
        const ekRef = REF_SPEED_AT_300K * REF_SPEED_AT_300K; // kuadrat kecepatan ref
        const ekCurrent = totalSpeed * totalSpeed;
        const ekPersen = Math.round((ekCurrent / ekRef) * 100);
        ekSpan.innerText = ekPersen + '%';
        
        // Update tampilan Kelvin
        kelvinSpan.innerText = Math.round(currentKelvin) + ' K';
        
        // Update slider tanpa memicu event berulang
        kelvinSlider.value = currentKelvin;
        
        // Tampilkan note khusus untuk 0 K
        if (currentKelvin === 0) {
            zeroNote.style.display = 'block';
        } else {
            zeroNote.style.display = 'none';
        }
    }

    // ==========  TOMBOL INTERAKSI ==========
    document.getElementById('tambahPartikelBtn').addEventListener('click', () => {
        setParticleCount(particles.length + 3);
    });
    document.getElementById('kurangPartikelBtn').addEventListener('click', () => {
        setParticleCount(particles.length - 3);
    });
    
    document.getElementById('resetBtn').addEventListener('click', () => {
        currentKelvin = 300;
        particles = initParticles(20, 300);
        targetCount = 20;
        
        // Update slider
        document.getElementById('kelvinSlider').value = 300;
        updateInfo();
    });

    // ==========  SLIDER SUHU KELVIN (0 - 500 K) ==========
    const kelvinSlider = document.getElementById('kelvinSlider');
    
    kelvinSlider.addEventListener('input', (e) => {
        const newKelvin = parseInt(e.target.value);
        applyKelvinTemperature(newKelvin);
    });

    // ==========  INTERAKSI DRAG (GAYA DORONG) ==========
    let isDragging = false;
    let lastMouseX = 0, lastMouseY = 0;

    function handleDragStart(e) {
        e.preventDefault();
        const rect = canvas.getBoundingClientRect();
        const scaleX = canvas.width / rect.width;
        const scaleY = canvas.height / rect.height;

        const mouseX = (e.clientX - rect.left) * scaleX;
        const mouseY = (e.clientY - rect.top) * scaleY;

        isDragging = true;
        lastMouseX = mouseX;
        lastMouseY = mouseY;
    }

    function handleDragMove(e) {
        if (!isDragging) return;
        e.preventDefault();

        const rect = canvas.getBoundingClientRect();
        const scaleX = canvas.width / rect.width;
        const scaleY = canvas.height / rect.height;

        const mouseX = (e.clientX - rect.left) * scaleX;
        const mouseY = (e.clientY - rect.top) * scaleY;

        const dx = mouseX - lastMouseX;
        const dy = mouseY - lastMouseY;

        if (Math.hypot(dx, dy) < 1) return;

        const impulseRadius = 70;
        const forceFactor = 0.6;

        particles.forEach(p => {
            const distX = p.x - mouseX;
            const distY = p.y - mouseY;
            const dist = Math.hypot(distX, distY);
            if (dist < impulseRadius) {
                const influence = (1 - dist / impulseRadius) * forceFactor; 
                p.vx += dx * influence;
                p.vy += dy * influence;
            }
        });

        lastMouseX = mouseX;
        lastMouseY = mouseY;
        updateInfo();
    }

    function handleDragEnd(e) {
        isDragging = false;
    }

    canvas.addEventListener('mousedown', handleDragStart);
    window.addEventListener('mousemove', handleDragMove);
    window.addEventListener('mouseup', handleDragEnd);
    canvas.addEventListener('dragstart', (e) => e.preventDefault());

    // ==========  LOOP UTAMA (GERAK & TUMBUKAN) ==========
    function updateParticles() {
        // tumbukan dengan dinding (elastis sempurna)
        for (let p of particles) {
            p.x += p.vx;
            p.y += p.vy;

            if (p.x - p.r < 0) {
                p.x = p.r;
                p.vx = -p.vx;
            } else if (p.x + p.r > W) {
                p.x = W - p.r;
                p.vx = -p.vx;
            }
            if (p.y - p.r < 0) {
                p.y = p.r;
                p.vy = -p.vy;
            } else if (p.y + p.r > H) {
                p.y = H - p.r;
                p.vy = -p.vy;
            }
        }

        // tumbukan antar partikel (sederhana)
        for (let i = 0; i < particles.length; i++) {
            for (let j = i + 1; j < particles.length; j++) {
                const p1 = particles[i];
                const p2 = particles[j];
                const dx = p2.x - p1.x;
                const dy = p2.y - p1.y;
                const dist = Math.hypot(dx, dy);
                const minDist = p1.r + p2.r;

                if (dist < minDist) {
                    const nx = dx / dist;
                    const ny = dy / dist;

                    const vxRel = p2.vx - p1.vx;
                    const vyRel = p2.vy - p1.vy;
                    const velAlong = vxRel * nx + vyRel * ny;

                    if (velAlong > 0) continue;

                    const impulse = 2 * velAlong / (1 + 1);
                    p1.vx += impulse * nx;
                    p1.vy += impulse * ny;
                    p2.vx -= impulse * nx;
                    p2.vy -= impulse * ny;

                    const overlap = minDist - dist;
                    const moveX = nx * overlap * 0.55;
                    const moveY = ny * overlap * 0.55;
                    p1.x -= moveX;
                    p1.y -= moveY;
                    p2.x += moveX;
                    p2.y += moveY;
                }
            }
        }
    }

    // ==========  GAMBAR ==========
    function draw() {
        ctx.clearRect(0, 0, W, H);

        ctx.strokeStyle = '#9bb6f0';
        ctx.lineWidth = 4;
        ctx.strokeRect(2, 2, W - 4, H - 4);

        for (let p of particles) {
            // Jika suhu 0 K, partikel berwarna kebiruan (dingin)
            let gradient;
            if (currentKelvin === 0) {
                gradient = ctx.createRadialGradient(p.x-3, p.y-3, 2, p.x, p.y, p.r+2);
                gradient.addColorStop(0, '#aaccff');
                gradient.addColorStop(0.6, '#5577aa');
                gradient.addColorStop(1, '#113355');
            } else {
                gradient = ctx.createRadialGradient(p.x-3, p.y-3, 2, p.x, p.y, p.r+2);
                gradient.addColorStop(0, '#ffd966');
                gradient.addColorStop(0.6, '#f2973a');
                gradient.addColorStop(1, '#c0561d');
            }
            
            ctx.beginPath();
            ctx.arc(p.x, p.y, p.r, 0, 2 * Math.PI);
            ctx.fillStyle = gradient;
            ctx.fill();
            ctx.shadowColor = 'white';
            ctx.shadowBlur = 10;
            ctx.fill();
            ctx.shadowBlur = 0;
            ctx.strokeStyle = '#fff6d0';
            ctx.lineWidth = 1.5;
            ctx.stroke();

            // ekor arah (hanya jika bergerak)
            if (Math.hypot(p.vx, p.vy) > 0.1) {
                ctx.beginPath();
                ctx.moveTo(p.x, p.y);
                ctx.lineTo(p.x - p.vx * 3, p.y - p.vy * 3);
                ctx.strokeStyle = '#fff9e6';
                ctx.lineWidth = 1;
                ctx.stroke();
            }
        }

        if (isDragging) {
            ctx.beginPath();
            ctx.arc(lastMouseX, lastMouseY, 70, 0, 2*Math.PI);
            ctx.strokeStyle = '#ffd966';
            ctx.setLineDash([8, 10]);
            ctx.lineWidth = 2;
            ctx.stroke();
            ctx.setLineDash([]);
        }

        updateInfo();
    }

    function animate() {
        updateParticles();
        draw();
        requestAnimationFrame(animate);
    }

    animate();
    updateInfo();
})();
</script>
</body>
</html>
