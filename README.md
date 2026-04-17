<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
    <title>TermoXplorer | Simulasi Gas Ideal & Termodinamika Interaktif</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Poppins', 'Segoe UI', system-ui, sans-serif;
        }

        body {
            background: linear-gradient(135deg, #0a0f2a 0%, #03081a 100%);
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 20px;
        }

        .app-container {
            width: 1400px;
            max-width: 98vw;
            background: rgba(8, 15, 28, 0.78);
            backdrop-filter: blur(14px);
            border-radius: 2.5rem;
            box-shadow: 0 30px 50px rgba(0,0,0,0.7), 0 0 0 2px rgba(255, 200, 100, 0.25);
            overflow: hidden;
        }

        .nav-buttons {
            display: flex;
            justify-content: center;
            gap: 1.2rem;
            padding: 1rem 1.8rem;
            background: #0b112ee6;
            border-bottom: 2px solid #ffc28555;
            flex-wrap: wrap;
        }

        .nav-btn {
            background: #1e2f44;
            border: none;
            padding: 10px 30px;
            border-radius: 60px;
            font-weight: 600;
            font-size: 1rem;
            color: #fef3d6;
            cursor: pointer;
            transition: 0.2s;
            box-shadow: 0 5px 10px rgba(0,0,0,0.3);
        }

        .nav-btn.active {
            background: #ffaa33;
            color: #0e1a2a;
            box-shadow: 0 0 15px #ffaa66;
        }

        .page {
            display: none;
            padding: 1.8rem;
            animation: fadeSlide 0.4s ease;
        }

        .page.active-page {
            display: block;
        }

        @keyframes fadeSlide {
            from { opacity: 0; transform: translateY(10px);}
            to { opacity: 1; transform: translateY(0);}
        }

        .sim-card {
            background: #0a1124cc;
            border-radius: 2rem;
            padding: 1.2rem;
        }

        .canvas-container {
            background: #010514;
            border-radius: 1.8rem;
            padding: 8px;
            box-shadow: inset 0 0 10px #00000055, 0 10px 20px black;
        }

        canvas {
            display: block;
            width: 100%;
            background: radial-gradient(circle at 20% 30%, #19233f, #030817);
            border-radius: 1.5rem;
            cursor: grab;
            border: 2px solid #ffcf8a;
        }
        canvas:active { cursor: grabbing; }

        .control-3col {
            display: flex;
            flex-wrap: wrap;
            gap: 20px;
            margin-top: 20px;
        }
        .ctrl-panel {
            flex: 1;
            background: #0e193ae0;
            border-radius: 1.6rem;
            padding: 18px;
            border: 1px solid #ffbc6e;
            backdrop-filter: blur(4px);
        }
        .ctrl-panel h3 {
            color: #ffe0aa;
            margin-bottom: 15px;
            font-size: 1.2rem;
            display: flex;
            align-items: center;
            gap: 10px;
        }
        .slider-group {
            margin-bottom: 18px;
        }
        .slider-group label {
            display: flex;
            justify-content: space-between;
            color: #ffefcf;
            font-weight: 500;
        }
        input[type=range] {
            width: 100%;
            margin-top: 8px;
            height: 6px;
            border-radius: 10px;
            background: linear-gradient(90deg, #2c7da0, #ffb347);
        }
        .stat-badge {
            background: #010a1b;
            border-radius: 1.2rem;
            padding: 12px;
            text-align: center;
            border-left: 4px solid #ffaa44;
        }
        .stats-row {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(120px,1fr));
            gap: 12px;
            margin-top: 18px;
        }
        .real-life-note {
            background: #2a2f45cc;
            border-radius: 1.5rem;
            padding: 15px 20px;
            margin-top: 20px;
            border: 1px solid #ffc285;
            color: #fff2df;
        }
        .materi-card {
            background: linear-gradient(145deg, #101b36, #09112a);
            border-radius: 2rem;
            padding: 2rem;
            color: #f5f0e6;
            box-shadow: 0 15px 25px rgba(0,0,0,0.4);
        }
        .rumus-box {
            background: #00000055;
            border-radius: 1.5rem;
            padding: 1rem;
            margin: 15px 0;
            font-family: monospace;
            font-size: 1.1rem;
            text-align: center;
        }
        .lkpd-container {
            background: #fffef7;
            border-radius: 2rem;
            padding: 2rem;
            color: #2c2a24;
        }
        .jawaban-siswa {
            width: 100%;
            padding: 12px;
            border-radius: 24px;
            border: 2px solid #f0a34b;
            margin: 10px 0;
            font-size: 0.95rem;
        }
        button {
            cursor: pointer;
            transition: 0.2s;
        }
        .btn-submit {
            background: #ff9f2e;
            border: none;
            padding: 12px 28px;
            border-radius: 40px;
            font-weight: bold;
        }
        .btn-submit:hover {
            background: #ffb347;
            transform: scale(1.02);
        }
        select {
            background: #ffefcf;
            border: none;
            padding: 8px 12px;
            border-radius: 40px;
            font-weight: bold;
        }
        .reset-btn {
            background: #6d4c2e;
            color: white;
            border: none;
            padding: 8px 18px;
            border-radius: 40px;
        }
        .daily-icon {
            font-size: 1.3rem;
            margin-right: 8px;
        }
        .warning-info {
            font-size: 0.75rem;
            color: #ffd966;
            margin-top: 5px;
        }
    </style>
</head>
<body>
<div class="app-container">
    <div class="nav-buttons">
        <button class="nav-btn active" data-page="simulasi">🔥 SIMULASI GAS</button>
        <button class="nav-btn" data-page="materi">📖 MATERI + RUMUS</button>
        <button class="nav-btn" data-page="lkpd">📝 LKPD DIGITAL</button>
    </div>

    <!-- HALAMAN SIMULASI -->
    <div id="simulasi" class="page active-page">
        <div class="sim-card">
            <div class="canvas-container">
                <canvas id="gasCanvas" width="1150" height="520"></canvas>
            </div>

            <div class="control-3col">
                <div class="ctrl-panel">
                    <h3>📦 VOLUME RUANG (geser bebas)</h3>
                    <div class="slider-group">
                        <label>🔘 Atur Volume <span id="volumeValueLabel">100%</span></label>
                        <input type="range" id="volumeSlider" min="35" max="200" value="100" step="1">
                        <p class="warning-info">★ Geser → piston bergerak (Hukum Boyle: P ∝ 1/V). Contoh: Jarum suntik, balon ditekan.</p>
                    </div>
                </div>
                <div class="ctrl-panel">
                    <h3>⚙️ TEKANAN EKSTERNAL (atur bebas)</h3>
                    <div class="slider-group">
                        <label>🎛️ Tekanan dari luar <span id="extPressureLabel">1.00</span> atm (relatif)</label>
                        <input type="range" id="pressureSlider" min="0.2" max="3.5" value="1.0" step="0.02">
                        <p class="warning-info">★ Tekanan eksternal tinggi mendorong piston ke dalam. Analogi: pompa ban atau panci presto.</p>
                    </div>
                </div>
                <div class="ctrl-panel">
                    <h3>🌡️ SUHU & PARTIKEL</h3>
                    <div class="slider-group">
                        <label>🔥 Suhu (K) <span id="tempShow">350 K</span></label>
                        <input type="range" id="tempSlider" min="50" max="900" value="350" step="2">
                    </div>
                    <div class="slider-group">
                        <label>🧪 Jumlah Partikel <span id="partCountShow">60</span></label>
                        <input type="range" id="partikelSlider" min="15" max="150" value="60" step="2">
                    </div>
                    <div style="display: flex; gap: 8px; flex-wrap:wrap;">
                        <select id="particleTypeSelect">
                            <option value="mono">Monoatomik (He, Ne)</option>
                            <option value="di">Diatomik (O₂, N₂)</option>
                        </select>
                        <select id="elementSelect">
                            <option value="Helium">Helium (He)</option>
                            <option value="Neon">Neon (Ne)</option>
                            <option value="Oksigen">Oksigen (O₂)</option>
                            <option value="Nitrogen">Nitrogen (N₂)</option>
                        </select>
                        <button id="resetSimBtn" class="reset-btn">⟳ Reset</button>
                    </div>
                </div>
            </div>

            <div class="stats-row">
                <div class="stat-badge">🌀 Tekanan Gas Aktual<br><span id="pressureActual">---</span> a.u</div>
                <div class="stat-badge">📐 Volume (%)<br><span id="volumeActual">100</span> %</div>
                <div class="stat-badge">⚡ Kecepatan Rata-rata<br><span id="rmsSpeed">0</span> px/frame</div>
                <div class="stat-badge">💥 Energi Kinetik<br><span id="ekStat">0</span> a.u</div>
                <div class="stat-badge">🔁 Tumbukan/dtk<br><span id="collisionRate">0</span></div>
            </div>

            <div class="real-life-note">
                <h4>🌍 <u>SIMULASI INI SEPERTI KEJADIAN NYATA:</u></h4>
                <div style="display: flex; flex-wrap: wrap; gap: 18px; margin-top: 8px;">
                    <div><span class="daily-icon">🚗</span> <strong>Ban Mobil Panas</strong> : Suhu ↑ → Tekanan ↑ (Isokhorik)</div>
                    <div><span class="daily-icon">💨</span> <strong>Semprotan Deodoran</strong> : Gas memuai cepat → tekanan turun (Isotermal)</div>
                    <div><span class="daily-icon">🍲</span> <strong>Panci Presto</strong> : Volume tetap, suhu & tekanan naik</div>
                    <div><span class="daily-icon">🎈</span> <strong>Balon Udara Panas</strong> : Tekanan tetap, volume membesar (Isobarik)</div>
                    <div><span class="daily-icon">💉</span> <strong>Jarum Suntik</strong> : Tekan piston → volume mengecil, tekanan naik (Boyle)</div>
                </div>
                <p style="margin-top: 12px;">✨ <strong>Cobalah:</strong> Geser <strong>Volume</strong> atau <strong>Tekanan Eksternal</strong> secara mandiri, lihat piston bergerak! Hubungkan dengan rumus PV = nRT.</p>
            </div>
        </div>
    </div>

    <!-- HALAMAN MATERI -->
    <div id="materi" class="page">
        <div class="materi-card">
            <h2 style="color:#ffcd7e;">📘 Termodinamika & Gas Ideal (Kurikulum Merdeka)</h2>
            <div style="display: flex; flex-wrap: wrap; gap: 25px; margin-top: 20px;">
                <div style="flex:1.2;">
                    <h3>✨ Hukum Gas Ideal</h3>
                    <div class="rumus-box">
                        <strong>PV = nRT</strong><br>
                        P = Tekanan (Pa/atm), V = Volume (m³), n = jumlah mol,<br>
                        R = konstanta gas (8.314 J/mol·K), T = Suhu (Kelvin)
                    </div>
                    <h3>⚙️ Proses Termodinamika & Rumusnya</h3>
                    <ul style="margin-left: 1.2rem; line-height: 1.7;">
                        <li><strong>Isobarik</strong> (Tekanan Tetap) → V₁/T₁ = V₂/T₂ <br>Contoh: Balon udara, air mendidih di panci terbuka.</li>
                        <li><strong>Isokhorik</strong> (Volume Tetap) → P₁/T₁ = P₂/T₂ <br>Contoh: Ban mobil panas, pressure cooker.</li>
                        <li><strong>Isotermal</strong> (Suhu Tetap) → P₁V₁ = P₂V₂ (Hukum Boyle) <br>Contoh: Pompa sepeda, semprotan aerosol.</li>
                        <li><strong>Adiabatik</strong> (Tanpa kalor) → PV^γ = konstan (aplikasi: mesin diesel)</li>
                    </ul>
                </div>
                <div style="flex:1; background:#00000033; border-radius: 1.5rem; padding: 1.2rem;">
                    <h3>🔬 Penerapan Nyata dalam Kehidupan</h3>
                    <p>✔️ <strong>Kulkas & AC</strong> : Kompresi & ekspansi gas refrigeran.<br>
                    ✔️ <strong>Mesin Kendaraan</strong> : Siklus Otto (kompresi adiabatik, pembakaran isokhorik).<br>
                    ✔️ <strong>Meteorologi</strong> : Pemanasan udara menyebabkan ekspansi isobarik → angin.<br>
                    ✔️ <strong>Pernapasan</strong> : Paru-paru mengembang (volume ↑, tekanan ↓) → udara masuk.</p>
                    <div class="rumus-box" style="margin-top: 15px;">
                        💡 <strong>Energi Dalam Gas Ideal</strong><br>
                        ΔU = (3/2)nRΔT (monoatomik)<br>
                        ΔU = (5/2)nRΔT (diatomik)
                    </div>
                </div>
            </div>
        </div>
    </div>

    <!-- HALAMAN LKPD -->
    <div id="lkpd" class="page">
        <div class="lkpd-container">
            <h2>📄 LKPD - Eksplorasi Gas Ideal & Termodinamika</h2>
            <p><strong>Capaian Pembelajaran (CP)</strong> : Siswa mampu menyelidiki dan menyimpulkan hubungan P, V, T gas ideal melalui simulasi serta mengaitkannya dengan fenomena sehari-hari.</p>
            <hr style="margin: 15px 0;">
            <textarea id="rumusanMasalah" rows="2" class="jawaban-siswa" placeholder="📌 Rumusan Masalah (contoh: Bagaimana pengaruh perubahan volume terhadap tekanan gas pada suhu tetap?)"></textarea>
            <textarea id="hipotesis" rows="2" class="jawaban-siswa" placeholder="🔬 Hipotesis / Dugaan sementara (gunakan rumus Boyle/Charles)"></textarea>
            <textarea id="dataObservasi" rows="3" class="jawaban-siswa" placeholder="📊 Data & Observasi: Catat nilai tekanan, volume, suhu saat slider diubah."></textarea>
            <textarea id="kesimpulan" rows="3" class="jawaban-siswa" placeholder="💡 Kesimpulan: Hubungan P, V, T dan kaitkan dengan kehidupan sehari-hari"></textarea>
            <button id="saveLkpdBtn" class="btn-submit">💾 SIMPAN JAWABAN</button>
            <div id="liveStatus" style="background:#e9f5e9; border-radius: 1.5rem; padding: 12px; margin-top: 15px;">✅ Jawaban tersimpan di penyimpanan lokal</div>
            <button id="exportDataBtn" style="margin-top: 12px; background:#5f7f9e; border: none; padding: 10px 24px; border-radius: 60px; color:white;">📎 Ekspor Jawaban</button>
        </div>
    </div>
</div>

<script>
    (function(){
        // CANVAS
        const canvas = document.getElementById('gasCanvas');
        const ctx = canvas.getContext('2d');
        let width = 1150, height = 520;
        canvas.width = width; canvas.height = height;

        let particles = [];
        const baseRadius = 5;
        let tempKelvin = 350;
        let particleType = 'mono';
        let elementName = 'Helium';
        let massFactor = 1.0;

        let volumePercent = 100;
        let externalPressure = 1.0;
        let rightWallX = width;
        let wallVelocity = 0;
        
        let collisionCounter = 0;
        let lastCollisionUpdate = Date.now();
        let collisionRate = 0;
        const refSpeed300 = 7.5;

        function getSpeedScale() {
            return Math.sqrt(tempKelvin / 300) / Math.sqrt(massFactor);
        }

        function updateMassFromType() {
            if (particleType === 'mono') {
                if (elementName === 'Helium') massFactor = 1.0;
                else if (elementName === 'Neon') massFactor = 1.9;
                else massFactor = 1.2;
            } else {
                if (elementName === 'Oksigen') massFactor = 2.66;
                else if (elementName === 'Nitrogen') massFactor = 2.33;
                else massFactor = 2.0;
            }
            applyTemperatureToAll();
        }

        function applyTemperatureToAll() {
            const scale = getSpeedScale();
            const baseSpeedVal = refSpeed300 * scale;
            for (let p of particles) {
                if (tempKelvin <= 0) { p.vx = 0; p.vy = 0; continue; }
                let spd = Math.hypot(p.vx, p.vy);
                if (spd < 0.15) {
                    let ang = Math.random() * 2 * Math.PI;
                    let newSpd = baseSpeedVal * (0.7 + Math.random() * 0.9);
                    p.vx = Math.cos(ang) * newSpd;
                    p.vy = Math.sin(ang) * newSpd;
                } else {
                    let targetSpeed = baseSpeedVal * (0.8 + Math.random() * 0.8);
                    let ratio = targetSpeed / spd;
                    p.vx *= ratio; p.vy *= ratio;
                }
            }
            updateStats();
        }

        function initParticles(count, kelvin) {
            let arr = [];
            let curRight = rightWallX;
            let baseSpdRef = refSpeed300 * Math.sqrt(kelvin/300) / Math.sqrt(massFactor);
            for (let i = 0; i < count; i++) {
                let x = Math.random() * (curRight - 2*baseRadius) + baseRadius;
                let y = Math.random() * (height - 2*baseRadius) + baseRadius;
                let vx = 0, vy = 0;
                if (kelvin > 0) {
                    let ang = Math.random() * 2 * Math.PI;
                    let spd = baseSpdRef * (0.6 + Math.random() * 1.0);
                    vx = Math.cos(ang) * spd;
                    vy = Math.sin(ang) * spd;
                }
                arr.push({x, y, vx, vy, r: baseRadius});
            }
            return arr;
        }

        function setParticleCount(newCount) {
            newCount = Math.min(160, Math.max(15, newCount));
            let cur = particles.length;
            if (newCount > cur) {
                let scale = getSpeedScale();
                let baseS = refSpeed300 * scale;
                for(let i=0; i<newCount-cur; i++) {
                    let x = Math.random() * (rightWallX - 2*baseRadius) + baseRadius;
                    let y = Math.random() * (height - 2*baseRadius) + baseRadius;
                    let vx=0,vy=0;
                    if(tempKelvin>0){
                        let ang=Math.random()*2*Math.PI;
                        let spd=baseS*(0.7+Math.random()*0.8);
                        vx=Math.cos(ang)*spd; vy=Math.sin(ang)*spd;
                    }
                    particles.push({x,y,vx,vy,r:baseRadius});
                }
            } else if (newCount < cur) {
                particles.splice(newCount, cur - newCount);
            }
            document.getElementById('partCountShow').innerText = particles.length;
            updateStats();
        }

        function applyPressureAndMoveWall() {
            let currentRight = rightWallX;
            let impulseSum = 0;
            for(let p of particles) {
                if(p.x + p.r > currentRight - 3 && p.vx > 0) {
                    impulseSum += Math.abs(p.vx) * massFactor * 1.2;
                    collisionCounter++;
                }
            }
            let internalPressure = (impulseSum / (particles.length+1)) * (tempKelvin/350) * 0.7;
            let netForce = internalPressure - externalPressure;
            let wallAcc = netForce * 0.35;
            wallVelocity += wallAcc;
            wallVelocity *= 0.94;
            let newRight = currentRight + wallVelocity;
            let minW = width * 0.35;
            let maxW = width * 2.0;
            if(newRight < minW) { newRight = minW; wallVelocity = 0; }
            if(newRight > maxW) { newRight = maxW; wallVelocity = 0; }
            rightWallX = newRight;
            let newVolPercent = (rightWallX / width) * 100;
            newVolPercent = Math.min(200, Math.max(35, newVolPercent));
            if(Math.abs(volumePercent - newVolPercent) > 0.5) {
                volumePercent = newVolPercent;
                document.getElementById('volumeSlider').value = volumePercent;
                document.getElementById('volumeValueLabel').innerText = Math.round(volumePercent)+'%';
                document.getElementById('volumeActual').innerText = Math.round(volumePercent)+'%';
            }
        }

        function handleCollisionsAndBoundaries() {
            let curRight = rightWallX;
            for (let p of particles) {
                p.x += p.vx;
                p.y += p.vy;
                if(p.x - p.r < 0) { p.x = p.r; p.vx = -p.vx; collisionCounter++; }
                if(p.x + p.r > curRight) { p.x = curRight - p.r; p.vx = -p.vx; collisionCounter++; }
                if(p.y - p.r < 0) { p.y = p.r; p.vy = -p.vy; collisionCounter++; }
                if(p.y + p.r > height) { p.y = height - p.r; p.vy = -p.vy; collisionCounter++; }
            }
            for(let i=0;i<particles.length;i++){
                for(let j=i+1;j<particles.length;j++){
                    let p1=particles[i], p2=particles[j];
                    let dx=p2.x-p1.x, dy=p2.y-p1.y;
                    let dist=Math.hypot(dx,dy);
                    let minD=p1.r+p2.r;
                    if(dist<minD){
                        let nx=dx/dist, ny=dy/dist;
                        let vrelx=p2.vx-p1.vx, vrely=p2.vy-p1.vy;
                        let velAlong=vrelx*nx+vrely*ny;
                        if(velAlong<0){
                            let impulse=2*velAlong/(1+1);
                            p1.vx+=impulse*nx; p1.vy+=impulse*ny;
                            p2.vx-=impulse*nx; p2.vy-=impulse*ny;
                            collisionCounter++;
                        }
                        let overlap=minD-dist;
                        let moveX=nx*overlap*0.55, moveY=ny*overlap*0.55;
                        p1.x-=moveX; p1.y-=moveY;
                        p2.x+=moveX; p2.y+=moveY;
                    }
                }
            }
        }

        function updateStats() {
            let totalSpeed = 0;
            for(let p of particles) totalSpeed += Math.hypot(p.vx, p.vy);
            let avgSpeed = particles.length ? totalSpeed/particles.length : 0;
            document.getElementById('rmsSpeed').innerText = avgSpeed.toFixed(2);
            let ek = avgSpeed*avgSpeed * massFactor;
            document.getElementById('ekStat').innerText = ek.toFixed(1);
            let pressureVal = (particles.length * (tempKelvin/300)) * (avgSpeed/3.5) * (100/volumePercent);
            pressureVal = Math.min(7.5, pressureVal).toFixed(2);
            document.getElementById('pressureActual').innerHTML = pressureVal + " a.u";
            document.getElementById('tempShow').innerText = tempKelvin + " K";
            document.getElementById('partCountShow').innerText = particles.length;
            document.getElementById('volumeActual').innerText = Math.round(volumePercent)+'%';
            document.getElementById('volumeValueLabel').innerText = Math.round(volumePercent)+'%';
            document.getElementById('extPressureLabel').innerText = externalPressure.toFixed(2);
            
            let now = Date.now();
            if(now - lastCollisionUpdate > 800) {
                collisionRate = Math.floor(collisionCounter / 0.8);
                collisionCounter = 0;
                lastCollisionUpdate = now;
            }
            document.getElementById('collisionRate').innerHTML = collisionRate;
        }

        // DRAG
        let dragging=false, dragX=0, dragY=0;
        function getCoord(e) {
            const rect=canvas.getBoundingClientRect();
            const sx=canvas.width/rect.width, sy=canvas.height/rect.height;
            let cx,cy;
            if(e.touches){ cx=e.touches[0].clientX; cy=e.touches[0].clientY; }
            else { cx=e.clientX; cy=e.clientY; }
            return {x:(cx-rect.left)*sx, y:(cy-rect.top)*sy};
        }
        function onDragStart(e){ e.preventDefault(); dragging=true; let p=getCoord(e); dragX=p.x; dragY=p.y; }
        function onDragMove(e){ if(!dragging) return; e.preventDefault(); let p=getCoord(e); let dx=p.x-dragX, dy=p.y-dragY; if(Math.hypot(dx,dy)>0.2){ for(let part of particles){ let dist=Math.hypot(part.x-p.x, part.y-p.y); if(dist<85){ let force=(1-dist/85)*1.1; part.vx+=dx*force; part.vy+=dy*force; } } } dragX=p.x; dragY=p.y; updateStats();}
        function onDragEnd(e){ dragging=false; }
        canvas.addEventListener('mousedown',onDragStart); window.addEventListener('mousemove',onDragMove); window.addEventListener('mouseup',onDragEnd);
        canvas.addEventListener('touchstart',onDragStart); window.addEventListener('touchmove',onDragMove); window.addEventListener('touchend',onDragEnd);

        function draw() {
            ctx.clearRect(0,0,width,height);
            let curRight = rightWallX;
            ctx.strokeStyle="#ffcc77";
            ctx.lineWidth=3;
            ctx.strokeRect(6,6,curRight-12,height-12);
            ctx.fillStyle = "#bd7f3ad9";
            ctx.fillRect(curRight-12, 0, 14, height);
            ctx.fillStyle = "#f3bc6c";
            ctx.fillRect(curRight-10, 0, 10, height);
            
            for(let p of particles){
                let grad=ctx.createRadialGradient(p.x-3,p.y-3,2,p.x,p.y,p.r+3);
                if(tempKelvin>500) grad.addColorStop(0,'#ff9f66'),grad.addColorStop(1,'#cc4411');
                else if(tempKelvin<180) grad.addColorStop(0,'#7bc5ff'),grad.addColorStop(1,'#2266cc');
                else grad.addColorStop(0,'#ffcd7e'),grad.addColorStop(1,'#e0872c');
                ctx.beginPath(); ctx.arc(p.x,p.y,p.r,0,Math.PI*2);
                ctx.fillStyle=grad; ctx.fill();
                ctx.shadowBlur=7; ctx.fill(); ctx.shadowBlur=0;
                ctx.strokeStyle='#fff6e5'; ctx.lineWidth=1; ctx.stroke();
            }
            if(dragging){ ctx.beginPath(); ctx.arc(dragX,dragY,72,0,Math.PI*2); ctx.strokeStyle='#ffcc88'; ctx.setLineDash([6,10]); ctx.stroke(); ctx.setLineDash([]);}
            updateStats();
        }

        function animate() {
            applyPressureAndMoveWall();
            handleCollisionsAndBoundaries();
            draw();
            requestAnimationFrame(animate);
        }

        // INIT
        particles = initParticles(60, 350);
        
        document.getElementById('tempSlider').addEventListener('input', (e)=>{ tempKelvin = parseInt(e.target.value); applyTemperatureToAll(); updateStats(); });
        document.getElementById('partikelSlider').addEventListener('input', (e)=>{ setParticleCount(parseInt(e.target.value)); });
        document.getElementById('volumeSlider').addEventListener('input', (e)=>{ 
            volumePercent = parseFloat(e.target.value); 
            let newR = width*(volumePercent/100); 
            rightWallX = Math.min(width*2, Math.max(width*0.35, newR)); 
            wallVelocity = 0; 
            document.getElementById('volumeActual').innerText = Math.round(volumePercent)+'%';
            document.getElementById('volumeValueLabel').innerText = Math.round(volumePercent)+'%';
        });
        document.getElementById('pressureSlider').addEventListener('input', (e)=>{ 
            externalPressure = parseFloat(e.target.value); 
            document.getElementById('extPressureLabel').innerText = externalPressure.toFixed(2); 
        });
        document.getElementById('particleTypeSelect').onchange = (e)=>{ particleType = e.target.value; updateMassFromType(); };
        document.getElementById('elementSelect').onchange = (e)=>{ elementName = e.target.value; updateMassFromType(); };
        document.getElementById('resetSimBtn').onclick = ()=>{
            tempKelvin = 350; externalPressure = 1.0; volumePercent = 100;
            rightWallX = width; wallVelocity=0;
            document.getElementById('tempSlider').value = 350;
            document.getElementById('pressureSlider').value = 1.0;
            document.getElementById('volumeSlider').value = 100;
            document.getElementById('extPressureLabel').innerText = "1.00";
            document.getElementById('volumeValueLabel').innerText = "100%";
            document.getElementById('volumeActual').innerText = "100%";
            setParticleCount(60);
            applyTemperatureToAll();
            updateStats();
        };
        
        // LKPD
        document.getElementById('saveLkpdBtn').onclick = () => {
            const data = {
                rumusan: document.getElementById('rumusanMasalah').value,
                hipotesis: document.getElementById('hipotesis').value,
                observasi: document.getElementById('dataObservasi').value,
                kesimpulan: document.getElementById('kesimpulan').value
            };
            localStorage.setItem('lkpdGasIdeal', JSON.stringify(data));
            document.getElementById('liveStatus').innerHTML = '✅ Jawaban tersimpan! (' + new Date().toLocaleTimeString() + ')';
        };
        document.getElementById('exportDataBtn').onclick = () => {
            const saved = localStorage.getItem('lkpdGasIdeal');
            if(saved) {
                const data = JSON.parse(saved);
                const exportText = `RUMUSAN MASALAH:\n${data.rumusan}\n\nHIPOTESIS:\n${data.hipotesis}\n\nDATA OBSERVASI:\n${data.observasi}\n\nKESIMPULAN:\n${data.kesimpulan}`;
                const blob = new Blob([exportText], {type: 'text/plain'});
                const a = document.createElement('a');
                const url = URL.createObjectURL(blob);
                a.href = url; a.download = 'lkpd_termodinamika.txt';
                a.click(); URL.revokeObjectURL(url);
            } else {
                alert('Belum ada data yang tersimpan.');
            }
        };
        
        // Navigasi
        const btns = document.querySelectorAll('.nav-btn');
        const pages = document.querySelectorAll('.page');
        btns.forEach(btn => {
            btn.addEventListener('click', () => {
                const pageId = btn.getAttribute('data-page');
                btns.forEach(b => b.classList.remove('active'));
                btn.classList.add('active');
                pages.forEach(page => page.classList.remove('active-page'));
                document.getElementById(pageId).classList.add('active-page');
            });
        });
        
        animate();
    })();
</script>
</body>
</html>

