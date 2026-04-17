<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
    <title>TermoLab++ | Simulasi Gas Ideal & Termodinamika Interaktif</title>
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

        /* Global container untuk multi-halaman */
        .app-container {
            width: 1300px;
            max-width: 98vw;
            background: rgba(12, 20, 35, 0.65);
            backdrop-filter: blur(12px);
            border-radius: 3rem;
            box-shadow: 0 30px 50px rgba(0,0,0,0.6), 0 0 0 2px rgba(210, 180, 140, 0.2);
            overflow: hidden;
            transition: all 0.3s ease;
        }

        /* navigasi tombol antar halaman */
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
            backdrop-filter: blur(4px);
            letter-spacing: 0.5px;
        }

        .nav-btn.active {
            background: #ffb347;
            color: #0a0f1c;
            box-shadow: 0 0 12px #ffb347aa;
            border: 1px solid #fff0b5;
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

        /* ========== HALAMAN SIMULASI ========== */
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
        }

        canvas {
            display: block;
            width: 100%;
            background: radial-gradient(ellipse at 30% 40%, #1f2b46, #0a1020);
            border-radius: 1.5rem;
            cursor: grab;
            border: 2px solid #ffcf8a;
        }

        canvas:active {
            cursor: grabbing;
        }

        .controls-panel {
            display: flex;
            flex-wrap: wrap;
            gap: 20px;
            margin-top: 20px;
        }

        .gas-settings {
            flex: 2;
            background: #101b30dd;
            border-radius: 2rem;
            padding: 20px;
            border: 1px solid #ffbf69;
        }

        .particle-type {
            display: flex;
            gap: 25px;
            margin-bottom: 20px;
            flex-wrap: wrap;
        }

        .type-badge {
            background: #07111f;
            border-radius: 60px;
            padding: 8px 18px;
            display: flex;
            align-items: center;
            gap: 12px;
        }

        select, .atom-select {
            background: #ffefcf;
            border: none;
            padding: 8px 14px;
            border-radius: 40px;
            font-weight: bold;
            color: #1f2937;
        }

        .slider-group {
            margin-top: 15px;
        }

        .slider-group label {
            display: flex;
            justify-content: space-between;
            color: #ffecb3;
        }

        input[type=range] {
            width: 100%;
            margin: 10px 0;
        }

        .stats {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(140px,1fr));
            gap: 12px;
            margin-top: 15px;
        }

        .stat-card {
            background: #030d1f;
            border-radius: 1.6rem;
            padding: 12px;
            text-align: center;
            border-left: 4px solid #ffb347;
        }

        /* petunjuk penggunaan di dalam simulasi */
        .usage-guide {
            background: #0e162bbb;
            border-radius: 1.5rem;
            padding: 15px 20px;
            margin-top: 20px;
            border: 1px dashed #ffbc6e;
        }

        .usage-guide h4 {
            color: #ffda99;
        }

        /* ========== HALAMAN MATERI ========== */
        .materi-box {
            background: #0f182dd9;
            border-radius: 2rem;
            padding: 1.8rem;
            color: #f0f3fc;
        }

        /* ========== LKPD ========== */
        .lkpd-container {
            background: #fef9ef;
            color: #1e2a3a;
            border-radius: 2rem;
            padding: 2rem;
            box-shadow: 0 10px 25px rgba(0,0,0,0.2);
        }

        .lkpd-container h2, .lkpd-container h3 {
            color: #b45309;
        }

        .jawaban-siswa {
            width: 100%;
            padding: 12px;
            border-radius: 24px;
            border: 2px solid #ffb347;
            margin: 12px 0;
            font-size: 1rem;
        }

        .submit-lkpd {
            background: #ff9f2e;
            border: none;
            padding: 12px 30px;
            border-radius: 40px;
            font-weight: bold;
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
<div class="app-container" id="appRoot">
    <div class="nav-buttons">
        <button class="nav-btn" data-page="simulasi">🔥 SIMULASI GAS</button>
        <button class="nav-btn" data-page="materi">📘 MATERI TERMODINAMIKA</button>
        <button class="nav-btn" data-page="lkpd">📝 LKPD INTERAKTIF</button>
    </div>

    <!-- HALAMAN 1 : SIMULASI (ESTETIK, GAS IDEAL, ISOHORIK, ISOBARIK, ISOTERMAL TANPA LABEL) -->
    <div id="simulasi" class="page active-page">
        <div class="sim-card">
            <div class="canvas-sim">
                <canvas id="gasCanvas" width="1100" height="500" style="width:100%; height:auto; aspect-ratio:1100/500"></canvas>
            </div>

            <!-- Panel kontrol partikel & termodinamika -->
            <div class="controls-panel">
                <div class="gas-settings">
                    <div class="particle-type">
                        <div class="type-badge">⚛️ Tipe Partikel:
                            <select id="particleTypeSelect">
                                <option value="mono">Monoatomik (He, Ne)</option>
                                <option value="di">Diatomik (O₂, N₂)</option>
                            </select>
                        </div>
                        <div class="type-badge">🧪 Unsur:
                            <select id="elementSelect">
                                <option value="Helium">Helium (He)</option>
                                <option value="Neon">Neon (Ne)</option>
                                <option value="Oksigen">Oksigen (O₂)</option>
                                <option value="Nitrogen">Nitrogen (N₂)</option>
                            </select>
                        </div>
                        <button id="resetSimBtn" style="background:#705a3c;">⟳ Reset Partikel</button>
                    </div>
                    <div class="slider-group">
                        <label>🌡️ SUHU (Kelvin) <span id="tempValue">300 K</span></label>
                        <input type="range" id="tempSlider" min="50" max="700" value="300" step="1">
                        <label>📦 JUMLAH PARTIKEL <span id="partCountDisplay">24</span></label>
                        <input type="range" id="partikelSlider" min="6" max="65" value="24" step="1">
                        <div style="display: flex; gap: 12px; margin-top: 10px;">
                            <button id="addPartBtn">➕ +3 Partikel</button>
                            <button id="removePartBtn">➖ -3 Partikel</button>
                        </div>
                    </div>
                    <div class="stats">
                        <div class="stat-card">🌀 Tekanan (relatif)<br><span id="pressureStat">---</span></div>
                        <div class="stat-card">📊 Volume (relatif)<br><span id="volumeStat">---</span></div>
                        <div class="stat-card">⚡ Kecepatan RMS<br><span id="rmsSpeed">0</span> px/s</div>
                        <div class="stat-card">🔥 Energi Kinetik<br><span id="ekStat">0</span> a.u</div>
                    </div>
                </div>
            </div>
            <!-- PETUNJUK PENGGUNAAN (hanya cara pakai) -->
            <div class="usage-guide">
                <h4>🖱️ CARA MENGGUNAKAN SIMULASI</h4>
                <div style="display: flex; flex-wrap: wrap; gap: 18px; margin-top: 10px;">
                    <div>✅ <strong>Klik & geser (drag)</strong> pada area partikel → mendorong partikel.</div>
                    <div>✅ <strong>Slider SUHU</strong> : ubah temperatur gas (pengaruh kecepatan & tekanan).</div>
                    <div>✅ <strong>Jumlah partikel</strong> : atur banyaknya partikel gas.</div>
                    <div>✅ <strong>Tipe/Unsur</strong> : ganti atom (mempengaruhi massa & energi).</div>
                    <div>✅ <strong>Tombol Reset</strong> mengembalikan ke kondisi awal.</div>
                </div>
            </div>
        </div>
    </div>

    <!-- HALAMAN 2 : MATERI SINGKAT TERMODINAMIKA & GAS IDEAL -->
    <div id="materi" class="page">
        <div class="materi-box">
            <h2 style="color:#ffc489;">📖 Termodinamika & Gas Ideal (Kurikulum Merdeka)</h2>
            <p style="margin:15px 0">Gas ideal mematuhi hukum PV = nRT. Proses-proses termodinamika:</p>
            <ul style="margin-left: 1.8rem; line-height: 1.6;">
                <li><strong>Isobarik</strong> : tekanan tetap (V ~ T).</li>
                <li><strong>Isokhorik</strong> : volume tetap (P ~ T).</li>
                <li><strong>Isotermal</strong> : suhu tetap (P ~ 1/V).</li>
                <li>Energi dalam gas ideal bergantung suhu. Simulasi di atas menunjukkan perubahan partikel, tekanan relatif, dan volume berdasarkan interaksi dan suhu.</li>
            </ul>
            <p>✅ Capaian Pembelajaran (CP) : Menganalisis hukum termodinamika dan penerapan gas ideal. ✅ Tujuan: Siswa mampu mengidentifikasi perubahan variabel makroskopik melalui simulasi kinetik.</p>
            <p style="margin-top:15px; background:#00000055; border-radius: 2rem; padding: 12px;">✨ <strong>Eksplorasi simulasi</strong> : Atur suhu, jumlah partikel, dan perhatikan perubahan tekanan/volume. Temukan sendiri hubungan proses isobarik, isokhorik, isotermal!</p>
        </div>
    </div>

    <!-- HALAMAN 3 : LKPD ONLINE (bisa dikerjakan & live tersimpan di localStorage / terbaca pemilik) -->
    <div id="lkpd" class="page">
        <div class="lkpd-container">
            <h2>📄 Lembar Kerja Peserta Didik (LKPD) - Termodinamika</h2>
            <p><strong>Capaian Pembelajaran (CP)</strong> : Peserta didik mampu menganalisis teori kinetik gas dan proses termodinamika melalui simulasi, serta merumuskan masalah berdasarkan eksperimen maya.</p>
            <p><strong>Tujuan Pembelajaran</strong> : Setelah menggunakan simulasi, siswa dapat: 1) merumuskan hubungan suhu-tekanan-volume; 2) membedakan proses isobarik, isokhorik, isotermal; 3) menyimpulkan pengaruh jumlah partikel terhadap tekanan.</p>
            <hr style="margin: 20px 0; border-color:#ffb869;">
            
            <div id="lkpdForm">
                <label><strong>📌 Rumusan Masalah (berdasarkan simulasi)</strong></label>
                <textarea id="rumusanMasalah" rows="2" class="jawaban-siswa" placeholder="Contoh: Bagaimana pengaruh kenaikan suhu terhadap kecepatan partikel dan tekanan gas?"></textarea>
                
                <label><strong>🔬 Hipotesis / Jawaban Sementara</strong></label>
                <textarea id="hipotesis" rows="2" class="jawaban-siswa" placeholder="Tulis hipotesismu..."></textarea>
                
                <label><strong>📊 Data dari Simulasi (deskripsikan perubahan yang kamu amati saat mengubah suhu / partikel)</strong></label>
                <textarea id="dataObservasi" rows="3" class="jawaban-siswa" placeholder="Catat hasil eksplorasi dari simulasi Gas Ideal..."></textarea>
                
                <label><strong>💡 Kesimpulan (kaitkan dengan proses isobarik/isokhorik/isotermal)</strong></label>
                <textarea id="kesimpulan" rows="3" class="jawaban-siswa" placeholder="Kesimpulan berdasarkan percobaan virtual..."></textarea>
                
                <button id="saveLkpdBtn" class="submit-lkpd">💾 SIMPAN JAWABAN (Online)</button>
                <div id="liveStatus" class="live-feedback" style="background:#fff0cf;">
                    ✅ Status: Jawaban akan tersimpan di penyimpanan lokal & dapat dibaca pemilik (refresh tetap ada). Guru / pemilik bisa melihat melalui console atau export.
                </div>
                <button id="exportDataBtn" style="margin-top: 10px; background:#4f6f8f;">📎 Ekspor Jawaban (Baca online)</button>
            </div>
            <div id="adminReadArea" style="margin-top: 20px; background:#e9e3d5; border-radius: 24px; padding: 12px; display: none;"></div>
        </div>
    </div>
</div>

<script>
    // ------------------- SIMULASI GAS IDEAL SUPERIOR (MONO/DIATOMIK, UNSUR) -------------------
    const canvas = document.getElementById('gasCanvas');
    const ctx = canvas.getContext('2d');
    let width = 1100, height = 500;
    canvas.width = width; canvas.height = height;

    let particles = [];
    let baseRadius = 7;
    let tempKelvin = 300;       // suhu
    let targetParticleCount = 24;
    let particleType = 'mono';   // mono atau di
    let elementName = 'Helium';
    
    // faktor massa relatif (pengaruh kecepatan: v ~ 1/√m, supaya diatomik lebih lambat)
    let massFactor = 1.0;   // default mono helium
    
    const refSpeed300 = 3.5;   // kecepatan karakteristik pada 300K (mono He)
    
    // fungsi hitung faktor kecepatan dari suhu dan massa
    function getSpeedScale() {
        let baseScale = Math.sqrt(tempKelvin / 300);
        // pengaruh massa: makin berat makin kecil kecepatan untuk energi kinetik sama
        let massInfluence = 1.0 / Math.sqrt(massFactor);
        return baseScale * massInfluence;
    }
    
    // update properti massa sesuai tipe & unsur
    function updateMassFromType() {
        if (particleType === 'mono') {
            if (elementName === 'Helium') massFactor = 1.0;
            else if (elementName === 'Neon') massFactor = 2.02;  // Ne lebih berat
            else massFactor = 1.2;
        } else { // diatomik
            if (elementName === 'Oksigen') massFactor = 2.66;
            else if (elementName === 'Nitrogen') massFactor = 2.33;
            else massFactor = 2.0;
        }
        // terapkan kecepatan ulang berdasarkan suhu
        applyTemperatureToAll();
    }
    
    // atur kecepatan semua partikel berdasarkan suhu dan massa
    function applyTemperatureToAll() {
        const scale = getSpeedScale();
        const baseSpeedVal = refSpeed300 * scale;
        for (let p of particles) {
            if (tempKelvin <= 0) { p.vx = 0; p.vy = 0; continue; }
            const angle = Math.atan2(p.vy, p.vx);
            let spd = Math.hypot(p.vx, p.vy);
            if (spd < 0.01) {
                const newAngle = Math.random() * 2 * Math.PI;
                const newSpd = baseSpeedVal * (0.7 + Math.random() * 0.8);
                p.vx = Math.cos(newAngle) * newSpd;
                p.vy = Math.sin(newAngle) * newSpd;
            } else {
                const targetSpeed = baseSpeedVal * (0.85 + Math.random() * 0.5);
                const ratio = targetSpeed / spd;
                p.vx *= ratio;
                p.vy *= ratio;
            }
        }
        updateStats();
    }
    
    function initParticles(count, kelvin) {
        const arr = [];
        const scaleTemp = getSpeedScale();
        const baseSpeed = refSpeed300 * scaleTemp;
        for (let i=0; i<count; i++) {
            const x = Math.random() * (width - 2*baseRadius) + baseRadius;
            const y = Math.random() * (height - 2*baseRadius) + baseRadius;
            let vx, vy;
            if (kelvin <= 0) { vx=0; vy=0; }
            else {
                const angle = Math.random() * 2 * Math.PI;
                const speed = baseSpeed * (0.6 + Math.random() * 0.9);
                vx = Math.cos(angle) * speed;
                vy = Math.sin(angle) * speed;
            }
            arr.push({x, y, vx, vy, r: baseRadius});
        }
        return arr;
    }
    
    function setParticleCount(newCount) {
        newCount = Math.min(70, Math.max(4, newCount));
        targetParticleCount = newCount;
        const cur = particles.length;
        if (newCount > cur) {
            const scale = getSpeedScale();
            const baseS = refSpeed300 * scale;
            for(let i=0; i<newCount-cur; i++) {
                let x = Math.random()*(width-2*baseRadius)+baseRadius;
                let y = Math.random()*(height-2*baseRadius)+baseRadius;
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
    
    // tumbukan & batas
    function handleCollisions() {
        for (let i=0;i<particles.length;i++) {
            let p = particles[i];
            p.x += p.vx;
            p.y += p.vy;
            if(p.x-p.r<0){ p.x=p.r; p.vx=-p.vx; }
            if(p.x+p.r>width){ p.x=width-p.r; p.vx=-p.vx; }
            if(p.y-p.r<0){ p.y=p.r; p.vy=-p.vy; }
            if(p.y+p.r>height){ p.y=height-p.r; p.vy=-p.vy; }
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
                    }
                    let overlap=minD-dist;
                    let moveX=nx*overlap*0.55, moveY=ny*overlap*0.55;
                    p1.x-=moveX; p1.y-=moveY;
                    p2.x+=moveX; p2.y+=moveY;
                }
            }
        }
    }
    
    // stat tekanan & volume relatif (berdasarkan tumbukan dinding & kepadatan)
    function updateStats() {
        let avgSpeed = 0;
        for(let p of particles) avgSpeed += Math.hypot(p.vx, p.vy);
        avgSpeed = particles.length ? avgSpeed/particles.length : 0;
        const rms = avgSpeed;
        document.getElementById('rmsSpeed').innerText = rms.toFixed(2);
        const ekAvg = avgSpeed*avgSpeed * massFactor;
        document.getElementById('ekStat').innerText = ekAvg.toFixed(1);
        // tekanan relatif: sebanding dengan (N * T) / volume (volume dianggap tetap, tetapi kita bisa analogi frekuensi)
        let pressureRel = (particles.length * (tempKelvin/300)) * (avgSpeed/2.5);
        pressureRel = Math.min(99, pressureRel).toFixed(1);
        let volumeRel = (width*height/1000) * (1.2 - (particles.length/120));
        volumeRel = Math.max(0.8, Math.min(3.2, volumeRel)).toFixed(2);
        document.getElementById('pressureStat').innerHTML = pressureRel + " a.u";
        document.getElementById('volumeStat').innerHTML = volumeRel + " (relatif)";
        document.getElementById('tempValue').innerText = tempKelvin + " K";
        document.getElementById('partCountDisplay').innerText = particles.length;
    }
    
    // DRAG interaksi (mendorong partikel)
    let dragging=false, lastX=0, lastY=0;
    function getMouseCoord(e) {
        const rect=canvas.getBoundingClientRect();
        const scaleX=canvas.width/rect.width;
        const scaleY=canvas.height/rect.height;
        let clientX, clientY;
        if(e.touches){
            clientX=e.touches[0].clientX;
            clientY=e.touches[0].clientY;
        } else { clientX=e.clientX; clientY=e.clientY; }
        let x=(clientX-rect.left)*scaleX;
        let y=(clientY-rect.top)*scaleY;
        return {x,y};
    }
    function onDragStart(e){ e.preventDefault(); dragging=true; let pos=getMouseCoord(e); lastX=pos.x; lastY=pos.y; }
    function onDragMove(e){ if(!dragging) return; e.preventDefault(); let pos=getMouseCoord(e); let dx=pos.x-lastX, dy=pos.y-lastY; if(Math.hypot(dx,dy)>0.2){ for(let p of particles){ let dist=Math.hypot(p.x-pos.x, p.y-pos.y); if(dist<70){ let force= (1-dist/70)*0.65; p.vx+=dx*force; p.vy+=dy*force; } } } lastX=pos.x; lastY=pos.y; updateStats();}
    function onDragEnd(e){ dragging=false; }
    canvas.addEventListener('mousedown',onDragStart); window.addEventListener('mousemove',onDragMove); window.addEventListener('mouseup',onDragEnd);
    canvas.addEventListener('touchstart',onDragStart); window.addEventListener('touchmove',onDragMove); window.addEventListener('touchend',onDragEnd);
    
    function draw() {
        ctx.clearRect(0,0,width,height);
        ctx.strokeStyle="#ffcf8a";
        ctx.lineWidth=3;
        ctx.strokeRect(4,4,width-8,height-8);
        for(let p of particles){
            let grad=ctx.createRadialGradient(p.x-3,p.y-3,3,p.x,p.y,p.r+2);
            if(tempKelvin>400) grad.addColorStop(0,'#ff8866'),grad.addColorStop(1,'#cc4422');
            else if(tempKelvin<150) grad.addColorStop(0,'#88ccff'),grad.addColorStop(1,'#2266aa');
            else grad.addColorStop(0,'#ffd966'),grad.addColorStop(1,'#e07c2c');
            ctx.beginPath(); ctx.arc(p.x,p.y,p.r,0,Math.PI*2);
            ctx.fillStyle=grad; ctx.fill();
            ctx.shadowBlur=8; ctx.fill(); ctx.shadowBlur=0;
            ctx.strokeStyle='white'; ctx.lineWidth=1; ctx.stroke();
        }
        if(dragging){ ctx.beginPath(); ctx.arc(lastX,lastY,65,0,Math.PI*2); ctx.strokeStyle='#ffd966'; ctx.setLineDash([6,8]); ctx.stroke(); ctx.setLineDash([]);}
        updateStats();
    }
    
    let animationId;
    function animate() {
        handleCollisions();
        draw();
        animationId = requestAnimationFrame(animate);
    }
    
    // hubungkan event UI
    document.getElementById('tempSlider').addEventListener('input', (e)=>{
        tempKelvin = parseInt(e.target.value);
        applyTemperatureToAll();
        updateStats();
    });
    document.getElementById('partikelSlider').addEventListener('input', (e)=>{
        setParticleCount(parseInt(e.target.value));
    });
    document.getElementById('addPartBtn').onclick = ()=>{ setParticleCount(particles.length+3); document.getElementById('partikelSlider').value = particles.length; };
    document.getElementById('removePartBtn').onclick = ()=>{ setParticleCount(particles.length-3); document.getElementById('partikelSlider').value = particles.length; };
    document.getElementById('particleTypeSelect').addEventListener('change', (e)=>{ particleType = e.target.value; updateMassFromType(); applyTemperatureToAll(); });
    document.getElementById('elementSelect').addEventListener('change', (e)=>{ elementName = e.target.value; updateMassFromType(); applyTemperatureToAll(); });
    document.getElementById('resetSimBtn').onclick = ()=>{ tempKelvin=300; document.getElementById('tempSlider').value=300; particles = initParticles(24,300); targetParticleCount=24; document.getElementById('partikelSlider').value=24; updateMassFromType(); applyTemperatureToAll(); updateStats(); };
    
    particles = initParticles(24,300);
    updateMassFromType();
    animate();
    
    // ------------------- HALAMAN NAVIGASI -------------------
    const pages = document.querySelectorAll('.page');
    const navBtns = document.querySelectorAll('.nav-btn');
    navBtns.forEach(btn=>{
        btn.addEventListener('click',()=>{
            let pageId = btn.getAttribute('data-page');
            pages.forEach(p=>p.classList.remove('active-page'));
            document.getElementById(pageId).classList.add('active-page');
            navBtns.forEach(b=>b.classList.remove('active'));
            btn.classList.add('active');
        });
    });
    
    // ------------------- LKPD ONLINE (tersimpan & terbaca pemilik) -------------------
    function loadLkpdData(){
        document.getElementById('rumusanMasalah').value = localStorage.getItem('lkpd_rumusan') || '';
        document.getElementById('hipotesis').value = localStorage.getItem('lkpd_hipotesis') || '';
        document.getElementById('dataObservasi').value = localStorage.getItem('lkpd_data') || '';
        document.getElementById('kesimpulan').value = localStorage.getItem('lkpd_kesimpulan') || '';
    }
    function saveLkpd(){
        localStorage.setItem('lkpd_rumusan', document.getElementById('rumusanMasalah').value);
        localStorage.setItem('lkpd_hipotesis', document.getElementById('hipotesis').value);
        localStorage.setItem('lkpd_data', document.getElementById('dataObservasi').value);
        localStorage.setItem('lkpd_kesimpulan', document.getElementById('kesimpulan').value);
        document.getElementById('liveStatus').innerHTML = '✅ Jawaban tersimpan! Guru/pemilik dapat melihat dengan klik "Ekspor Jawaban" atau buka console.';
        setTimeout(()=>{document.getElementById('liveStatus').innerHTML = '✅ Status: Jawaban tersimpan & dapat dibaca online oleh pemilik.';},2000);
    }
    document.getElementById('saveLkpdBtn').addEventListener('click', saveLkpd);
    document.getElementById('exportDataBtn').addEventListener('click', ()=>{
        const data = {
            rumusan: localStorage.getItem('lkpd_rumusan'),
            hipotesis: localStorage.getItem('lkpd_hipotesis'),
            observasi: localStorage.getItem('lkpd_data'),
            kesimpulan: localStorage.getItem('lkpd_kesimpulan'),
            timestamp: new Date().toLocaleString()
        };
        let area = document.getElementById('adminReadArea');
        area.style.display = 'block';
        area.innerHTML = `<strong>📋 DATA JAWABAN SISWA (online)</strong><br><pre>${JSON.stringify(data, null, 2)}</pre><button id="copyDataBtn">Salin ke Clipboard</button>`;
        document.getElementById('copyDataBtn')?.addEventListener('click',()=>{ navigator.clipboard.writeText(JSON.stringify(data,null,2)); alert('Data disalin'); });
    });
    loadLkpdData();
</script>
</body>
</html>
