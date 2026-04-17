Maaf atas pemotongan kode pada jawaban sebelumnya. Berikut adalah kode **lengkap** yang sudah diperbaiki dan siap digunakan. Kode ini mencakup semua fitur yang Anda minta: navigasi antar halaman, simulasi dengan banyak partikel, kontrol volume & tekanan yang bisa diatur siswa, materi estetik, serta LKPD online.
```html
<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
    <title>TermoXplorer | Simulasi Gas Ideal & Termodinamika</title>
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
            background: rgba(8, 15, 28, 0.75);
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
    </style>
</head>
<body>
<div class="app-container">
    <div class="nav-buttons">
        <button class="nav-btn active" data-page="simulasi">🔥 SIMULASI GAS</button>
        <button class="nav-btn" data-page="materi">📖 MATERI + KEHIDUPAN</button>
        <button class="nav-btn" data-page="lkpd">📝 LKPD DIGITAL</button>
    </div>

    <!-- HALAMAN SIMULASI -->
    <div id="simulasi" class="page active-page">
        <div class="sim-card">
            <div class="canvas-container">
                <canvas id="gasCanvas" width="1150" height="520" style="width:100%; aspect-ratio:1150/520"></canvas>
            </div>

            <div class="control-3col">
                <div class="ctrl-panel">
                    <h3>📦 VOLUME RUANG (atur langsung)</h3>
                    <div class="slider-group">
                        <label>🔘 Geser Volume <span id="volumeValueLabel">100%</span></label>
                        <input type="range" id="volumeSlider" min="35" max="200" value="100" step="1">
                        <p style="font-size:0.75rem; color:#ddc">★ Piston bergerak, gas memuai/mampat</p>
                    </div>
                </div>
                <div class="ctrl-panel">
                    <h3>⚙️ TEKANAN EKSTERNAL (atur bebas)</h3>
                    <div class="slider-group">
                        <label>🎛️ Tekanan luar <span id="extPressureLabel">1.00</span> atm (relatif)</label>
                        <input type="range" id="pressureSlider" min="0.2" max="3.2" value="1.0" step="0.02">
                        <p style="font-size:0.75rem;">★ Mempengaruhi gerak dinding & keseimbangan gas</p>
                    </div>
                </div>
                <div class="ctrl-panel">
                    <h3>🌡️ SUHU & PARTIKEL</h3>
                    <div class="slider-group">
                        <label>🔥 Suhu (K) <span id="tempShow">350 K</span></label>
                        <input type="range" id="tempSlider" min="50" max="900" value="350" step="2">
                    </div>
                    <div class="slider-group">
                        <label>🧪 Jumlah Partikel <span id="partCountShow">55</span></label>
                        <input type="range" id="partikelSlider" min="15" max="140" value="55" step="2">
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
                <h4>🌍 <u>Hubungan dengan Kehidupan Sehari-hari</u></h4>
                <div style="display: flex; flex-wrap: wrap; gap: 18px; margin-top: 8px;">
                    <div>🚗 <strong>Ban Mobil</strong> : Suhu naik → tekanan naik.</div>
                    <div>💨 <strong>Semprotan aerosol</strong> : Gas memuai cepat → tekanan turun.</div>
                    <div>🍲 <strong>Panci Presto</strong> : Volume tetap, suhu & tekanan meningkat.</div>
                    <div>🎈 <strong>Balon udara panas</strong> : Isobarik, volume membesar karena suhu.</div>
                </div>
                <p style="margin-top: 8px;">✨ <strong>Coba atur volume, tekanan, dan suhu! Amati gerak partikel yang hidup dengan jumlah partikel banyak.</strong></p>
            </div>
        </div>
    </div>

    <!-- HALAMAN MATERI -->
    <div id="materi" class="page">
        <div class="materi-card">
            <h2 style="color:#ffcd7e;">📘 Termodinamika & Gas Ideal dalam Kehidupan</h2>
            <div style="display: flex; flex-wrap: wrap; gap: 25px; margin-top: 20px;">
                <div style="flex:1; min-width: 200px;">
                    <h3>✨ Hukum Gas Ideal</h3>
                    <p>PV = nRT. Gas ideal: partikel bergerak acak, tumbukan elastis, tanpa gaya antar partikel.</p>
                    <h3 style="margin-top: 20px;">⚙️ Proses Termodinamika</h3>
                    <ul style="margin-left: 1.2rem;">
                        <li><strong>Isobarik</strong> (tekanan tetap) : V/T = konstan → balon udara.</li>
                        <li><strong>Isokhorik</strong> (volume tetap) : P/T = konstan → pemanasan ban.</li>
                        <li><strong>Isotermal</strong> (suhu tetap) : PV = konstan → pompa sepeda.</li>
                    </ul>
                </div>
                <div style="flex:1; background:#00000033; border-radius: 1.5rem; padding: 1.2rem;">
                    <h3>🔬 Aplikasi Nyata</h3>
                    <p>✔️ Mesin pembakaran dalam (siklus Otto)<br>
                    ✔️ Kulkas & AC → kompresi & ekspansi gas<br>
                    ✔️ Meteorologi: Tekanan udara & suhu<br>
                    ✔️ Scuba diving: Hukum Boyle</p>
                    <p><strong>Capaian Pembelajaran (Kurikulum Merdeka)</strong> : Menganalisis hubungan tekanan, volume, dan suhu gas ideal serta penerapannya.</p>
                </div>
            </div>
            <div style="margin-top: 25px; background:#f1c38f30; border-radius: 2rem; padding: 15px;">
                <p>💡 <strong>Tips simulasi</strong> : mainkan slider Volume dan Tekanan eksternal untuk melihat piston bergerak. Semakin banyak partikel, semakin hidup geraknya!</p>
            </div>
        </div>
    </div>

    <!-- HALAMAN LKPD -->
    <div id="lkpd" class="page">
        <div class="lkpd-container">
            <h2>📄 LKPD - Eksplorasi Gas Ideal & Termodinamika</h2>
            <p><strong>Capaian Pembelajaran (CP)</strong> : Siswa mampu menyelidiki dan menyimpulkan hubungan makroskopik gas ideal melalui simulasi.</p>
            <p><strong>Tujuan</strong> : 1) Merumuskan hubungan P, V, T. 2) Mengidentifikasi proses isobarik, isokhorik, isotermal. 3) Mengaitkan dengan fenomena sehari-hari.</p>
            <hr style="margin: 15px 0;">
            <textarea id="rumusanMasalah" rows="2" class="jawaban-siswa" placeholder="📌 Rumusan Masalah (contoh: Bagaimana pengaruh perubahan volume terhadap tekanan gas pada suhu tetap?)"></textarea>
            <textarea id="hipotesis" rows="2" class="jawaban-siswa" placeholder="🔬 Hipotesis / Dugaan sementara"></textarea>
            <textarea id="dataObservasi" rows="3" class="jawaban-siswa" placeholder="📊 Data & Observasi dari simulasi (catat perubahan partikel, tekanan, volume, suhu)"></textarea>
            <textarea id="kesimpulan" rows="3" class="jawaban-siswa" placeholder="💡 Kesimpulan & kaitkan dengan kehidupan sehari-hari"></textarea>
            <button id="saveLkpdBtn" class="btn-submit">💾 SIMPAN JAWABAN</button>
            <div id="liveStatus" style="background:#e9f5e9; border-radius: 1.5rem; padding: 12px; margin-top: 15px;">✅ Jawaban tersimpan di penyimpanan lokal (bisa diekspor guru)</div>
            <button id="exportDataBtn" style="margin-top: 12px; background:#5f7f9e; border: none; padding: 10px 24px; border-radius: 60px; color:white;">📎 Ekspor Jawaban (Baca Online)</button>
            <div id="adminReadArea" style="margin-top: 18px; background:#f1e9da; border-radius: 24px; padding: 12px; display: none;"></div>
        </div>
    </div>
</div>

<script>
    // ======================= SIMULASI GAS LENGKAP =======================
    const canvas = document.getElementById('gasCanvas');
    const ctx = canvas.getContext('2d');
    let width = 1150, height = 520;
    canvas.width = width; canvas.height = height;

    let particles = [];
    let baseRadius = 5;
    let tempKelvin = 350;
    let targetCount = 55;
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
    
    const refSpeed300 = 7.2;  // lebih cepat & lincah
    
    function getSpeedScale() {
        let baseScale = Math.sqrt(tempKelvin / 300);
        let massInfluence = 1.0 / Math.sqrt(massFactor);
        return baseScale * massInfluence;
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
    
    function updateWallFromVolume() {
        let effWidth = width * (volumePercent / 100);
        effWidth = Math.min(width * 2.0, Math.max(width * 0.35, effWidth));
        rightWallX = effWidth;
        return rightWallX;
    }
    
    function initParticles(count, kelvin) {
        let arr = [];
        let curRight = updateWallFromVolume();
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
        newCount = Math.min(150, Math.max(15, newCount));
        let cur = particles.length;
        let curRight = rightWallX;
        if (newCount > cur) {
            let scale = getSpeedScale();
            let baseS = refSpeed300 * scale;
            for(let i=0; i<newCount-cur; i++) {
                let x = Math.random() * (curRight - 2*baseRadius) + baseRadius;
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
        let internalPressure = (impulseSum / (particles.length+1)) * (tempKelvin/350) * 0.65;
        let netForce = internalPressure - externalPressure;
        let wallAcc = netForce * 0.32;
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
        if(Math.abs(volumePercent - newVolPercent) > 0.6) {
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
        pressureVal = Math.min(7.2, pressureVal).toFixed(2);
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
    
    // Event Listeners
    document.getElementById('tempSlider').addEventListener('input', (e)=>{ tempKelvin = parseInt(e.target.value); applyTemperatureToAll(); updateStats(); });
    document.getElementById('partikelSlider').addEventListener('input', (e)=>{ setParticleCount(parseInt(e.target.value)); });
    document.getElementById('volumeSlider').addEventListener('input', (e)=>{ volumePercent = parseFloat(e.target.value); let newR = width*(volumePercent/100); rightWallX = Math.min(width*2, Math.max(width*0.35, newR)); wallVelocity = 0; document.getElementById('volumeActual').innerText = Math.round(volumePercent)+'%'; });
    document.getElementById('pressureSlider').addEventListener('input', (e)=>{ externalPressure = parseFloat(e.target.value); document.getElementById('extPressureLabel').innerText = externalPressure.toFixed(2); });
    document.getElementById('particleTypeSelect').onchange = (e)=>{ particleType = e.target.value; updateMassFromType(); };
    document.getElementById('elementSelect').onchange = (e)=>{ elementName = e.target.value; updateMassFromType(); };
    document.getElementById('resetSimBtn').onclick = ()=>{
        tempKelvin = 350; externalPressure = 1.0; volumePercent = 100;
        rightWallX = width; wallVelocity=0;
        document.getElementById('tempSlider').value=350;
        document.getElementById('pressureSlider').value=1.0;
        document.getElementById('volumeSlider').value=100;
        particles = initParticles(55,350);
        targetCount=55; document.getElementById('partikelSlider').value=55;
        updateMassFromType();
        applyTemperatureToAll();
        updateStats();
    };
    
    particles = initParticles(55,350);
    updateMassFromType();
    animate();
    
    // NAVIGASI HALAMAN
    const pages = document.querySelectorAll('.page');
    const navBtns = document.querySelectorAll('.nav-btn');
    navBtns.forEach(btn => {
        btn.addEventListener('click', () => {
            let id = btn.getAttribute('data-page');
            pages.forEach(p => p.classList.remove('active-page'));
            document.getElementById(id).classList.add('active-page');
            navBtns.forEach(b => b.classList.remove('active'));
            btn.classList.add('active');
        });
    });
    
    // LKPD STORAGE
    function loadLkpd(){
        document.getElementById('rumusanMasalah').value = localStorage.getItem('lkpd_rum') || '';
        document.getElementById('hipotesis').value = localStorage.getItem('lkpd_hyp') || '';
        document.getElementById('dataObservasi').value = localStorage.getItem('lkpd_data') || '';
        document.getElementById('kesimpulan').value = localStorage.getItem('lkpd_conc') || '';
    }
    function saveLkpd(){
        localStorage.setItem('lkpd_rum', document.getElementById('rumusanMasalah').value);
        localStorage.setItem('lkpd_hyp', document.getElementById('hipotesis').value);
        localStorage.setItem('lkpd_data', document.getElementById('dataObservasi').value);
        localStorage.setItem('lkpd_conc', document.getElementById('kesimpulan').value);
        document.getElementById('liveStatus').innerHTML = '✅ Jawaban tersimpan! Guru bisa lihat melalui ekspor.';
        setTimeout(()=>{document.getElementById('liveStatus').innerHTML = '✅ Jawaban tersimpan di penyimpanan lokal (bisa diekspor guru)';},2000);
    }
    document.getElementById('saveLkpdBtn').addEventListener('click', saveLkpd);
    document.getElementById('exportDataBtn').addEventListener('click', ()=>{
        let data = { 
            rumusan: localStorage.getItem('lkpd_rum'), 
            hipotesis: localStorage.getItem('lkpd_hyp'), 
            observasi: localStorage.getItem('lkpd_data'), 
            kesimpulan: localStorage.getItem('lkpd_conc'), 
            waktu: new Date().toLocaleString() 
        };
        let area = document.getElementById('adminReadArea');
        area.style.display = 'block';
        area.innerHTML = `<strong>📋 Data Jawaban Siswa (Online)</strong><pre style="background:white; padding:12px; border-radius:16px; overflow:auto;">${JSON.stringify(data, null, 2)}</pre><button id="copyDataBtn" style="margin-top:8px; background:#ffb347; border:none; padding:8px 16px; border-radius:40px;">📋 Salin ke Clipboard</button>`;
        document.getElementById('copyDataBtn')?.addEventListener('click', () => {
            navigator.clipboard.writeText(JSON.stringify(data, null, 2));
            alert('Data jawaban disalin ke clipboard!');
        });
    });
    loadLkpd();
</script>
</body>
</html>
```
