<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
    <title>TermoLab Pro | Simulasi Gas Ideal - Atur Volume & Tekanan</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Poppins', 'Segoe UI', system-ui, sans-serif;
        }

        body {
            background: radial-gradient(circle at 10% 30%, #0a0f2a, #030617);
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 20px;
        }

        .app-container {
            width: 1400px;
            max-width: 98vw;
            background: rgba(12, 20, 35, 0.65);
            backdrop-filter: blur(12px);
            border-radius: 3rem;
            box-shadow: 0 30px 50px rgba(0,0,0,0.6), 0 0 0 2px rgba(210, 180, 140, 0.2);
            overflow: hidden;
        }

        .nav-buttons {
            display: flex;
            justify-content: center;
            gap: 1.2rem;
            padding: 1rem 1.5rem;
            background: #0b1122cc;
            border-bottom: 1px solid #ffdf9c55;
            flex-wrap: wrap;
        }

        .nav-btn {
            background: #1e2b3c;
            border: none;
            padding: 10px 28px;
            border-radius: 60px;
            font-weight: 600;
            font-size: 1rem;
            color: #f0e6d0;
            cursor: pointer;
            transition: 0.2s;
            box-shadow: 0 4px 8px rgba(0,0,0,0.3);
        }

        .nav-btn.active {
            background: #ffb347;
            color: #0a0f1c;
            box-shadow: 0 0 12px #ffb347aa;
        }

        .page {
            display: none;
            padding: 1.5rem;
            animation: fadeIn 0.4s ease;
        }

        .page.active-page {
            display: block;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(8px);}
            to { opacity: 1; transform: translateY(0);}
        }

        .sim-card {
            background: #0d142ccc;
            border-radius: 2rem;
            padding: 1.2rem;
            backdrop-filter: blur(8px);
        }

        .canvas-sim {
            background: #03060f;
            border-radius: 2rem;
            padding: 6px;
            box-shadow: 0 12px 28px black;
            position: relative;
        }

        canvas {
            display: block;
            width: 100%;
            background: radial-gradient(ellipse at 30% 40%, #1f2b46, #0a1020);
            border-radius: 1.5rem;
            cursor: grab;
            border: 2px solid #ffcf8a;
        }
        canvas:active { cursor: grabbing; }

        .dual-control {
            display: flex;
            flex-wrap: wrap;
            gap: 20px;
            margin-top: 20px;
        }
        .control-group {
            flex: 1;
            background: #101b30dd;
            border-radius: 1.8rem;
            padding: 18px;
            border: 1px solid #ffbf69;
        }
        .control-group h3 {
            color: #ffda99;
            margin-bottom: 12px;
            font-size: 1.2rem;
        }
        .slider-group {
            margin-bottom: 18px;
        }
        .slider-group label {
            display: flex;
            justify-content: space-between;
            color: #ffecb3;
            margin-bottom: 6px;
        }
        input[type=range] {
            width: 100%;
            margin: 5px 0;
        }
        .stats-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(130px,1fr));
            gap: 12px;
            margin-top: 15px;
        }
        .stat-card {
            background: #030d1f;
            border-radius: 1.6rem;
            padding: 12px;
            text-align: center;
            border-left: 4px solid #ffb347;
            color: #f5e6d3;
        }
        .usage-guide {
            background: #0e162bbb;
            border-radius: 1.5rem;
            padding: 15px 20px;
            margin-top: 20px;
            border: 1px dashed #ffbc6e;
            color: #f0e6d0;
        }
        .materi-box {
            background: #0f182dd9;
            border-radius: 2rem;
            padding: 1.8rem;
            color: #f0f3fc;
        }
        .lkpd-container {
            background: #fef9ef;
            color: #1e2a3a;
            border-radius: 2rem;
            padding: 2rem;
        }
        .jawaban-siswa {
            width: 100%;
            padding: 12px;
            border-radius: 24px;
            border: 2px solid #ffb347;
            margin: 12px 0;
        }
        .submit-lkpd {
            background: #ff9f2e;
            border: none;
            padding: 12px 30px;
            border-radius: 40px;
            font-weight: bold;
            cursor: pointer;
        }
        button {
            cursor: pointer;
        }
        .live-feedback {
            background: #e9f5e9;
            padding: 15px;
            border-radius: 28px;
            margin-top: 20px;
        }
    </style>
</head>
<body>
<div class="app-container">
    <div class="nav-buttons">
        <button class="nav-btn active" data-page="simulasi">🔥 SIMULASI GAS</button>
        <button class="nav-btn" data-page="materi">📘 MATERI</button>
        <button class="nav-btn" data-page="lkpd">📝 LKPD</button>
    </div>

    <!-- HALAMAN SIMULASI -->
    <div id="simulasi" class="page active-page">
        <div class="sim-card">
            <div class="canvas-sim">
                <canvas id="gasCanvas" width="1100" height="500" style="width:100%; aspect-ratio:1100/500"></canvas>
            </div>

            <!-- Kontrol Ganda: Volume dan Tekanan dapat diatur siswa -->
            <div class="dual-control">
                <div class="control-group">
                    <h3>📦 KONTROL VOLUME (V)</h3>
                    <div class="slider-group">
                        <label>🔘 Luas Ruang <span id="volumeValueDisplay">100%</span></label>
                        <input type="range" id="volumeSlider" min="40" max="180" value="100" step="2">
                        <p style="font-size:0.8rem; color:#ccc;">*Geser untuk memperbesar/memperkecil ruang gas (efek isokhorik & isotermal)</p>
                    </div>
                </div>
                <div class="control-group">
                    <h3>⚙️ KONTROL TEKANAN EKSTERNAL (P)</h3>
                    <div class="slider-group">
                        <label>🎛️ Tekanan luar (relatif) <span id="externalPressureDisplay">1.00</span></label>
                        <input type="range" id="pressureSlider" min="0.2" max="2.8" value="1.0" step="0.02">
                        <p style="font-size:0.8rem; color:#ccc;">*Mempengaruhi gerak dinding & kompresi (efek isobarik)</p>
                    </div>
                </div>
                <div class="control-group">
                    <h3>🌡️ SUHU & PARTIKEL</h3>
                    <div class="slider-group">
                        <label>🔥 Suhu (K) <span id="tempValue">300 K</span></label>
                        <input type="range" id="tempSlider" min="50" max="800" value="300" step="1">
                    </div>
                    <div class="slider-group">
                        <label>🧪 Jumlah Partikel <span id="partCountDisplay">28</span></label>
                        <input type="range" id="partikelSlider" min="5" max="70" value="28" step="1">
                    </div>
                    <div style="display: flex; gap: 8px; flex-wrap:wrap;">
                        <select id="particleTypeSelect" style="background:#ffefcf; border-radius:40px; padding:6px 12px;">
                            <option value="mono">Monoatomik</option>
                            <option value="di">Diatomik</option>
                        </select>
                        <select id="elementSelect" style="background:#ffefcf; border-radius:40px; padding:6px 12px;">
                            <option value="Helium">Helium (He)</option>
                            <option value="Neon">Neon (Ne)</option>
                            <option value="Oksigen">Oksigen (O₂)</option>
                            <option value="Nitrogen">Nitrogen (N₂)</option>
                        </select>
                        <button id="resetSimBtn" style="background:#705a3c; border:none; padding:6px 18px; border-radius:40px;">⟳ Reset</button>
                    </div>
                </div>
            </div>

            <!-- Statistik实时 -->
            <div class="stats-grid">
                <div class="stat-card">🌀 Tekanan Gas (aktual)<br><span id="pressureActual">---</span> a.u</div>
                <div class="stat-card">📐 Volume Ruang<br><span id="volumeActual">100</span> %</div>
                <div class="stat-card">⚡ Kecepatan RMS<br><span id="rmsSpeed">0</span> px/frame</div>
                <div class="stat-card">💥 Energi Kinetik<br><span id="ekStat">0</span> a.u</div>
            </div>

            <div class="usage-guide">
                <h4>🖱️ CARA PAKAI SIMULASI (Klik & Eksplorasi)</h4>
                <div style="display: flex; flex-wrap: wrap; gap: 18px;">
                    <div>✅ <strong>Drag partikel</strong> → dorong dengan mouse</div>
                    <div>✅ <strong>Slider VOLUME</strong> : ubah luas ruang → gas memuai/terkompresi (dinding bergerak)</div>
                    <div>✅ <strong>Slider TEKANAN EKSTERNAL</strong> : atur tekanan dari luar (mempengaruhi gerak dinding)</div>
                    <div>✅ <strong>Suhu & Jumlah partikel</strong> : pengaruh kecepatan & tekanan</div>
                    <div>✅ <strong>Amati sendiri proses isobarik, isokhorik, isotermal!</strong></div>
                </div>
            </div>
        </div>
    </div>

    <!-- HALAMAN MATERI -->
    <div id="materi" class="page">
        <div class="materi-box">
            <h2 style="color:#ffc489;">📖 Termodinamika & Gas Ideal (Kurikulum Merdeka)</h2>
            <p>Simulasi ini memungkinkan siswa mengatur Volume dan Tekanan secara mandiri. Proses termodinamika:</p>
            <ul style="margin-left: 1.8rem; line-height: 1.6;">
                <li><strong>Isobarik</strong> : Tekanan tetap (atur tekanan eksternal konstan, ubah suhu/volume)</li>
                <li><strong>Isokhorik</strong> : Volume tetap (kunci volume dengan slider, ubah suhu)</li>
                <li><strong>Isotermal</strong> : Suhu tetap (atur suhu konstan, ubah volume dan amati tekanan)</li>
                <li>Hukum Boyle, Charles, Gay-Lussac dapat dieksplorasi.</li>
            </ul>
            <p>✅ CP: Menerapkan hukum termodinamika. ✅ Tujuan: Siswa mampu merumuskan hubungan P, V, T melalui simulasi interaktif.</p>
        </div>
    </div>

    <!-- HALAMAN LKPD -->
    <div id="lkpd" class="page">
        <div class="lkpd-container">
            <h2>📄 LKPD - Eksplorasi Gas Ideal</h2>
            <p><strong>Capaian Pembelajaran</strong> : Menganalisis pengaruh volume, tekanan, dan suhu terhadap gas ideal.</p>
            <p><strong>Tujuan</strong> : Siswa dapat menentukan hubungan antar variabel dan mengidentifikasi proses isobarik/isokhorik/isotermal.</p>
            <hr>
            <textarea id="rumusanMasalah" rows="2" class="jawaban-siswa" placeholder="Rumusan masalah (contoh: Bagaimana pengaruh perubahan volume terhadap tekanan saat suhu tetap?)"></textarea>
            <textarea id="hipotesis" rows="2" class="jawaban-siswa" placeholder="Hipotesis / jawaban sementara"></textarea>
            <textarea id="dataObservasi" rows="3" class="jawaban-siswa" placeholder="Data dari simulasi: catat perubahan P, V, T"></textarea>
            <textarea id="kesimpulan" rows="3" class="jawaban-siswa" placeholder="Kesimpulan & kaitkan dengan proses termodinamika"></textarea>
            <button id="saveLkpdBtn" class="submit-lkpd">💾 SIMPAN JAWABAN (Online)</button>
            <div id="liveStatus" class="live-feedback">✅ Tersimpan di lokal & bisa diekspor guru.</div>
            <button id="exportDataBtn" style="margin-top:12px; background:#4f6f8f; border:none; padding:10px 20px; border-radius:40px; color:white;">📎 Ekspor Jawaban (Baca online)</button>
            <div id="adminReadArea" style="margin-top: 20px; background:#e9e3d5; border-radius: 24px; padding: 12px; display: none;"></div>
        </div>
    </div>
</div>

<script>
    // ======================= SIMULASI DENGAN VOLUME & TEKANAN DINAMIS (DINDING BERGERAK) =======================
    const canvas = document.getElementById('gasCanvas');
    const ctx = canvas.getContext('2d');
    let width = 1100, height = 500;
    canvas.width = width; canvas.height = height;
    
    // Variabel gas
    let particles = [];
    let baseRadius = 6;
    let tempKelvin = 300;
    let targetCount = 28;
    let particleType = 'mono';
    let elementName = 'Helium';
    let massFactor = 1.0;
    
    // Volume control: persentase luas (40% - 180%) -> mempengaruhi batas kanan & kiri dinamis
    let volumePercent = 100;   // 100% = lebar normal 1100, tinggi 500
    let externalPressure = 1.0;  // tekanan eksternal (mempengaruhi dinding movable)
    
    // Dinding kiri tetap x=0, kanan bergerak sesuai volumePercent (simulasi piston)
    let rightWallX = width;      // akan diupdate setiap frame
    
    function updateWallFromVolume() {
        // volumePersen mempengaruhi lebar efektif: 40% -> 0.4*width, 180% -> 1.8*width
        let effectiveWidth = width * (volumePercent / 100);
        effectiveWidth = Math.min(width * 1.8, Math.max(width * 0.4, effectiveWidth));
        rightWallX = effectiveWidth;
        return rightWallX;
    }
    
    // Kecepatan referensi lebih cepat agar tidak lambat (dinaikkan)
    const refSpeed300 = 5.2;   // lebih lincah
    
    function getSpeedScale() {
        let baseScale = Math.sqrt(tempKelvin / 300);
        let massInfluence = 1.0 / Math.sqrt(massFactor);
        return baseScale * massInfluence;
    }
    
    function updateMassFromType() {
        if (particleType === 'mono') {
            if (elementName === 'Helium') massFactor = 1.0;
            else if (elementName === 'Neon') massFactor = 2.0;
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
            if (spd < 0.1) {
                let ang = Math.random() * 2 * Math.PI;
                let newSpd = baseSpeedVal * (0.8 + Math.random() * 0.9);
                p.vx = Math.cos(ang) * newSpd;
                p.vy = Math.sin(ang) * newSpd;
            } else {
                let targetSpeed = baseSpeedVal * (0.85 + Math.random() * 0.7);
                let ratio = targetSpeed / spd;
                p.vx *= ratio; p.vy *= ratio;
            }
        }
        updateStats();
    }
    
    function initParticles(count, kelvin) {
        let arr = [];
        let currentRight = updateWallFromVolume();
        for (let i=0; i<count; i++) {
            let x = Math.random() * (currentRight - 2*baseRadius) + baseRadius;
            let y = Math.random() * (height - 2*baseRadius) + baseRadius;
            let vx=0,vy=0;
            if(kelvin>0){
                let ang=Math.random()*2*Math.PI;
                let spd = refSpeed300 * Math.sqrt(kelvin/300) / Math.sqrt(massFactor) * (0.6+Math.random()*0.8);
                vx=Math.cos(ang)*spd; vy=Math.sin(ang)*spd;
            }
            arr.push({x,y,vx,vy,r:baseRadius});
        }
        return arr;
    }
    
    function setParticleCount(newCount) {
        newCount = Math.min(75, Math.max(4, newCount));
        let cur = particles.length;
        let currentRight = updateWallFromVolume();
        if (newCount > cur) {
            let scale = getSpeedScale();
            let baseS = refSpeed300 * scale;
            for(let i=0;i<newCount-cur;i++){
                let x = Math.random() * (currentRight - 2*baseRadius) + baseRadius;
                let y = Math.random() * (height - 2*baseRadius) + baseRadius;
                let vx=0,vy=0;
                if(tempKelvin>0){
                    let ang=Math.random()*2*Math.PI;
                    let spd=baseS*(0.7+Math.random()*0.7);
                    vx=Math.cos(ang)*spd; vy=Math.sin(ang)*spd;
                }
                particles.push({x,y,vx,vy,r:baseRadius});
            }
        } else if (newCount < cur) {
            particles.splice(newCount, cur - newCount);
        }
        document.getElementById('partCountDisplay').innerText = particles.length;
        updateStats();
    }
    
    // Gaya dari tekanan eksternal: dinding kanan mendapat gaya yang mendorong ke kiri sebanding externalPressure
    // dan tekanan internal dari tumbukan partikel.
    let wallVelocity = 0;
    function applyPressureAndMoveWall() {
        let currentRight = rightWallX;
        // Hitung tekanan internal: berdasarkan momentum transfer (sederhana)
        let internalPressure = 0;
        let impulseSum = 0;
        for(let p of particles) {
            // jika partikel mendekati dinding kanan dengan vx positif -> tumbukan elastis
            if(p.x + p.r > currentRight - 2 && p.vx > 0) {
                impulseSum += Math.abs(p.vx) * massFactor;
            }
        }
        internalPressure = (impulseSum / (particles.length+1)) * (tempKelvin/300) * 0.5;
        let netForce = internalPressure - externalPressure;
        // percepatan dinding
        let wallAcc = netForce * 0.28;
        wallVelocity += wallAcc;
        wallVelocity *= 0.96; // redaman
        let newRight = currentRight + wallVelocity;
        // batasi volume antara 40% dan 180%
        let minW = width * 0.4;
        let maxW = width * 1.8;
        if(newRight < minW) { newRight = minW; wallVelocity = 0; }
        if(newRight > maxW) { newRight = maxW; wallVelocity = 0; }
        rightWallX = newRight;
        // update volumePercent sesuai posisi dinding
        let newVolPercent = (rightWallX / width) * 100;
        newVolPercent = Math.min(180, Math.max(40, newVolPercent));
        if(Math.abs(volumePercent - newVolPercent) > 0.5) {
            volumePercent = newVolPercent;
            document.getElementById('volumeSlider').value = volumePercent;
            document.getElementById('volumeActual').innerText = Math.round(volumePercent)+'%';
            document.getElementById('volumeValueDisplay').innerText = Math.round(volumePercent)+'%';
        }
    }
    
    // Update batasan partikel terhadap dinding bergerak (kanan) dan kiri tetap 0
    function handleBoundariesAndCollisions() {
        let curRight = rightWallX;
        for (let p of particles) {
            p.x += p.vx;
            p.y += p.vy;
            // kiri x=0
            if(p.x - p.r < 0) { p.x = p.r; p.vx = -p.vx; }
            // kanan movable
            if(p.x + p.r > curRight) { p.x = curRight - p.r; p.vx = -p.vx; }
            if(p.y - p.r < 0) { p.y = p.r; p.vy = -p.vy; }
            if(p.y + p.r > height) { p.y = height - p.r; p.vy = -p.vy; }
        }
        // Tumbukan antar partikel
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
        let avgSpeed = 0;
        for(let p of particles) avgSpeed += Math.hypot(p.vx, p.vy);
        avgSpeed = particles.length ? avgSpeed/particles.length : 0;
        document.getElementById('rmsSpeed').innerText = avgSpeed.toFixed(2);
        let ek = avgSpeed*avgSpeed * massFactor;
        document.getElementById('ekStat').innerText = ek.toFixed(1);
        // Tekanan aktual dari simulasi
        let pressureVal = (particles.length * (tempKelvin/300)) * (avgSpeed/3.0) * (100/volumePercent);
        pressureVal = Math.min(5.5, pressureVal).toFixed(2);
        document.getElementById('pressureActual').innerHTML = pressureVal + " a.u";
        document.getElementById('tempValue').innerText = tempKelvin + " K";
        document.getElementById('partCountDisplay').innerText = particles.length;
        document.getElementById('volumeActual').innerText = Math.round(volumePercent)+'%';
        document.getElementById('volumeValueDisplay').innerText = Math.round(volumePercent)+'%';
        document.getElementById('externalPressureDisplay').innerText = externalPressure.toFixed(2);
    }
    
    // DRAG (dorong partikel)
    let dragging=false, lastDragX=0, lastDragY=0;
    function getCoord(e) {
        const rect=canvas.getBoundingClientRect();
        const scaleX=canvas.width/rect.width;
        const scaleY=canvas.height/rect.height;
        let cx, cy;
        if(e.touches){ cx=e.touches[0].clientX; cy=e.touches[0].clientY; }
        else { cx=e.clientX; cy=e.clientY; }
        return {x:(cx-rect.left)*scaleX, y:(cy-rect.top)*scaleY};
    }
    function onDragStart(e){ e.preventDefault(); dragging=true; let p=getCoord(e); lastDragX=p.x; lastDragY=p.y; }
    function onDragMove(e){ if(!dragging) return; e.preventDefault(); let p=getCoord(e); let dx=p.x-lastDragX, dy=p.y-lastDragY; if(Math.hypot(dx,dy)>0.3){ for(let part of particles){ let dist=Math.hypot(part.x-p.x, part.y-p.y); if(dist<75){ let force=(1-dist/75)*0.85; part.vx+=dx*force; part.vy+=dy*force; } } } lastDragX=p.x; lastDragY=p.y; updateStats();}
    function onDragEnd(e){ dragging=false; }
    canvas.addEventListener('mousedown',onDragStart); window.addEventListener('mousemove',onDragMove); window.addEventListener('mouseup',onDragEnd);
    canvas.addEventListener('touchstart',onDragStart); window.addEventListener('touchmove',onDragMove); window.addEventListener('touchend',onDragEnd);
    
    function draw() {
        ctx.clearRect(0,0,width,height);
        let curRight = rightWallX;
        ctx.strokeStyle="#ffcf8a";
        ctx.lineWidth=3;
        ctx.strokeRect(4,4,curRight-8,height-8);
        // Gambar piston kanan
        ctx.fillStyle = "#b97f44aa";
        ctx.fillRect(curRight-10, 0, 12, height);
        ctx.fillStyle = "#e7b874";
        ctx.fillRect(curRight-8, 0, 8, height);
        for(let p of particles){
            let grad=ctx.createRadialGradient(p.x-3,p.y-3,3,p.x,p.y,p.r+2);
            if(tempKelvin>450) grad.addColorStop(0,'#ff8866'),grad.addColorStop(1,'#cc4422');
            else if(tempKelvin<150) grad.addColorStop(0,'#88ccff'),grad.addColorStop(1,'#2266aa');
            else grad.addColorStop(0,'#ffd966'),grad.addColorStop(1,'#e07c2c');
            ctx.beginPath(); ctx.arc(p.x,p.y,p.r,0,Math.PI*2);
            ctx.fillStyle=grad; ctx.fill();
            ctx.shadowBlur=6; ctx.fill(); ctx.shadowBlur=0;
            ctx.strokeStyle='white'; ctx.lineWidth=1; ctx.stroke();
        }
        if(dragging){ ctx.beginPath(); ctx.arc(lastDragX,lastDragY,65,0,Math.PI*2); ctx.strokeStyle='#ffd966'; ctx.setLineDash([6,8]); ctx.stroke(); ctx.setLineDash([]);}
        updateStats();
    }
    
    function animate() {
        applyPressureAndMoveWall();   // dinding bergerak karena tekanan eksternal & internal
        handleBoundariesAndCollisions();
        draw();
        requestAnimationFrame(animate);
    }
    
    // Event UI
    document.getElementById('tempSlider').addEventListener('input', (e)=>{
        tempKelvin = parseInt(e.target.value);
        applyTemperatureToAll();
        updateStats();
    });
    document.getElementById('partikelSlider').addEventListener('input', (e)=>{ setParticleCount(parseInt(e.target.value)); });
    document.getElementById('volumeSlider').addEventListener('input', (e)=>{
        volumePercent = parseFloat(e.target.value);
        let newRight = width * (volumePercent/100);
        newRight = Math.min(width*1.8, Math.max(width*0.4, newRight));
        rightWallX = newRight;
        wallVelocity = 0;
        document.getElementById('volumeActual').innerText = Math.round(volumePercent)+'%';
        document.getElementById('volumeValueDisplay').innerText = Math.round(volumePercent)+'%';
    });
    document.getElementById('pressureSlider').addEventListener('input', (e)=>{
        externalPressure = parseFloat(e.target.value);
        document.getElementById('externalPressureDisplay').innerText = externalPressure.toFixed(2);
    });
    document.getElementById('particleTypeSelect').onchange = (e)=>{ particleType = e.target.value; updateMassFromType(); };
    document.getElementById('elementSelect').onchange = (e)=>{ elementName = e.target.value; updateMassFromType(); };
    document.getElementById('resetSimBtn').onclick = ()=>{
        tempKelvin = 300; externalPressure = 1.0; volumePercent = 100;
        rightWallX = width; wallVelocity=0;
        document.getElementById('tempSlider').value=300;
        document.getElementById('pressureSlider').value=1.0;
        document.getElementById('volumeSlider').value=100;
        particles = initParticles(28,300);
        targetCount=28; document.getElementById('partikelSlider').value=28;
        updateMassFromType();
        applyTemperatureToAll();
        updateStats();
    };
    
    particles = initParticles(28,300);
    updateMassFromType();
    animate();
    
    // NAVIGASI HALAMAN
    const pages = document.querySelectorAll('.page');
    const btns = document.querySelectorAll('.nav-btn');
    btns.forEach(btn=>{
        btn.addEventListener('click',()=>{
            let id = btn.getAttribute('data-page');
            pages.forEach(p=>p.classList.remove('active-page'));
            document.getElementById(id).classList.add('active-page');
            btns.forEach(b=>b.classList.remove('active'));
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
        document.getElementById('liveStatus').innerHTML = '✅ Jawaban tersimpan! Guru dapat ekspor.';
        setTimeout(()=>{document.getElementById('liveStatus').innerHTML = '✅ Tersimpan di lokal & bisa diekspor guru.';},2000);
    }
    document.getElementById('saveLkpdBtn').addEventListener('click', saveLkpd);
    document.getElementById('exportDataBtn').addEventListener('click',()=>{
        let data = {
            rumusan: localStorage.getItem('lkpd_rum'),
            hipotesis: localStorage.getItem('lkpd_hyp'),
            observasi: localStorage.getItem('lkpd_data'),
            kesimpulan: localStorage.getItem('lkpd_conc'),
        };
        let area = document.getElementById('adminReadArea');
        area.style.display = 'block';
        area.innerHTML = `<strong>📋 Jawaban Siswa:</strong><pre style="background:white;padding:12px;border-radius:16px;">${JSON.stringify(data,null,2)}</pre><button id="copyBtn">Salin</button>`;
        document.getElementById('copyBtn')?.addEventListener('click',()=>{ navigator.clipboard.writeText(JSON.stringify(data,null,2)); alert('Disalin'); });
    });
    loadLkpd();
</script>
</body>
</html>
