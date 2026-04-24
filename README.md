<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=yes">
    <title>TermoXplorer | Simulasi Gas Ideal + Isobarik + 3 LKPD</title>
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
            padding: 20px;
        }
        .app-container {
            width: 1400px;
            max-width: 98vw;
            background: rgba(8, 15, 28, 0.78);
            backdrop-filter: blur(14px);
            border-radius: 2.5rem;
            margin: 0 auto;
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
        .mode-selector {
            background: #1e2f44;
            border-radius: 60px;
            padding: 8px;
            display: flex;
            gap: 10px;
            margin-bottom: 15px;
        }
        .mode-btn {
            flex: 1;
            background: #2c4b6e;
            border: none;
            padding: 8px;
            border-radius: 40px;
            color: white;
            cursor: pointer;
            font-weight: bold;
            transition: 0.2s;
        }
        .mode-btn.active-mode {
            background: #ffaa33;
            color: #0e1a2a;
            box-shadow: 0 0 8px #ffaa66;
        }
        .stats-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(170px, 1fr));
            gap: 18px;
            margin-top: 24px;
            margin-bottom: 16px;
        }
        .stat-card {
            background: linear-gradient(145deg, #07132e, #030c1f);
            border-radius: 1.5rem;
            padding: 1rem 0.8rem;
            text-align: center;
            border: 1px solid #ffcf8a66;
            box-shadow: 0 8px 20px rgba(0,0,0,0.4);
        }
        .stat-label {
            font-size: 0.85rem;
            text-transform: uppercase;
            letter-spacing: 1.5px;
            font-weight: 600;
            color: #ffdd99;
            margin-bottom: 10px;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 8px;
        }
        .stat-value {
            font-size: 2rem;
            font-weight: 800;
            color: #ffefcf;
            background: #00000055;
            display: inline-block;
            padding: 8px 18px;
            border-radius: 60px;
            font-family: 'Courier New', monospace;
        }
        .stat-unit {
            font-size: 0.85rem;
            color: #ffbb77;
            margin-left: 5px;
        }
        .stat-desc {
            font-size: 0.7rem;
            color: #bbaa88;
            margin-top: 8px;
        }
        .real-life-note {
            background: #2a2f45cc;
            border-radius: 1.5rem;
            padding: 15px;
            margin-top: 20px;
            color: #fff2df;
        }
        .materi-card {
            background: linear-gradient(145deg, #101b36, #09112a);
            border-radius: 2rem;
            padding: 2rem;
            color: #f5f0e6;
        }
        .rumus-box {
            background: #00000055;
            border-radius: 1.5rem;
            padding: 1rem;
            margin: 15px 0;
            text-align: center;
            font-family: monospace;
            font-size: 1.1rem;
        }
        button {
            cursor: pointer;
            transition: 0.2s;
        }
        .reset-btn {
            background: #6d4c2e;
            color: white;
            border: none;
            padding: 8px 18px;
            border-radius: 40px;
        }
        .warning-info {
            font-size: 0.7rem;
            color: #ffd966;
            margin-top: 5px;
        }
        .lkpd-section-wrapper {
            background: #fffef7;
            border-radius: 2rem;
            padding: 0.2rem;
        }
        .tombol-lkpd-nav {
            display: flex;
            justify-content: center;
            gap: 1rem;
            margin-bottom: 1.5rem;
            flex-wrap: wrap;
            background: #f5ede1;
            padding: 1rem;
            border-radius: 60px;
        }
        .btn-lkpd {
            background: #2c4b6e;
            border: none;
            padding: 12px 24px;
            font-size: 1rem;
            font-weight: bold;
            border-radius: 40px;
            color: white;
            cursor: pointer;
            transition: 0.2s;
        }
        .btn-lkpd.active-lkpd {
            background: #ff9f3a;
            color: #1e2c1c;
            box-shadow: 0 0 10px #ffbb55;
        }
        .lkpd-page {
            display: none;
            animation: fadePage 0.3s ease;
        }
        .lkpd-page.active-lkpd-page {
            display: block;
        }
        @keyframes fadePage {
            from { opacity: 0; transform: translateY(8px);}
            to { opacity: 1; transform: translateY(0);}
        }
        .sheet-card {
            background: #ffffff;
            border-radius: 1.8rem;
            padding: 1.5rem;
            border: 1px solid #ffe0b5;
        }
        .tujuan-card {
            background: #e8f0fe;
            border-radius: 1.2rem;
            padding: 12px 18px;
            margin-bottom: 20px;
            border-left: 6px solid #ff9f2e;
            color: #1e3a5f;
        }
        .field-group {
            margin-bottom: 22px;
        }
        .field-group label {
            font-weight: 700;
            display: flex;
            align-items: center;
            gap: 10px;
            margin-bottom: 8px;
            color: #2c3e4e;
        }
        textarea {
            width: 100%;
            border-radius: 1.2rem;
            border: 2px solid #e2cfb3;
            padding: 12px 16px;
            background: #fffdf9;
            resize: vertical;
        }
        .tabel-observasi {
            background: #fef9ef;
            border-radius: 1.2rem;
            padding: 8px;
            border: 1px solid #ffd9a5;
            overflow-x: auto;
        }
        .tabel-observasi table {
            width: 100%;
            border-collapse: collapse;
        }
        .tabel-observasi th, .tabel-observasi td {
            border: 1px solid #d6c29b;
            padding: 10px 6px;
            text-align: center;
        }
        .tabel-observasi th {
            background: #ffeed5;
        }
        .editable-cell input {
            width: 100%;
            border: none;
            background: transparent;
            text-align: center;
            outline: none;
            font-size: 0.9rem;
        }
        .btn-simpan-lkpd {
            background: #2e7d64;
            border: none;
            padding: 10px 28px;
            border-radius: 60px;
            color: white;
            font-weight: bold;
            margin-top: 10px;
        }
        .info-guru {
            background: #e6f4ea;
            border-radius: 1rem;
            padding: 10px 15px;
            margin-top: 18px;
            font-size: 0.8rem;
            border-left: 5px solid #2b7a4b;
        }
        .flex-buttons {
            display: flex;
            gap: 12px;
            flex-wrap: wrap;
        }
        .btn-ekspor-semua {
            background: #4f6f8f;
        }
    </style>
</head>
<body>
<div class="app-container">
    <div class="nav-buttons">
        <button class="nav-btn active" data-page="simulasi">🔥 SIMULASI GAS</button>
        <button class="nav-btn" data-page="materi">📖 MATERI + RUMUS</button>
        <button class="nav-btn" data-page="lkpd">📝 LKPD DIGITAL (3 Lembar)</button>
    </div>

    <div id="simulasi" class="page active-page">
        <div class="sim-card">
            <div class="canvas-container">
                <canvas id="gasCanvas" width="1150" height="520"></canvas>
            </div>
            <div class="control-3col">
                <div class="ctrl-panel">
                    <h3>📦 VOLUME RUANG (Liter)</h3>
                    <div class="slider-group">
                        <label>🔘 Volume <span id="volumeValueLabel">2.00 L</span></label>
                        <input type="range" id="volumeSlider" min="0.5" max="4.0" value="2.0" step="0.05">
                        <p class="warning-info">★ Pada mode isobarik, volume akan otomatis menyesuaikan</p>
                    </div>
                </div>
                <div class="ctrl-panel">
                    <h3>⚙️ TEKANAN GAS</h3>
                    <div class="mode-selector">
                        <button id="modeAutoBtn" class="mode-btn active-mode">📈 Otomatis (PV=nRT)</button>
                        <button id="ModeIsobarikBtn" class="mode-btn">⚖️ Isobarik (Tekanan Tetap)</button>
                    </div>
                    <div class="slider-group" id="pressureSliderGroup">
                        <label>🎛️ Tekanan Tetap (Isobarik) <span id="fixedPressureLabel">1.00</span> atm</label>
                        <input type="range" id="fixedPressureSlider" min="0.3" max="3.0" value="1.0" step="0.02" disabled>
                        <p class="warning-info">★ Mode isobarik: tekanan dijaga konstan, volume menyesuaikan</p>
                    </div>
                    <div class="slider-group">
                        <label>📊 Tekanan Aktual <span id="pressureDisplay">1.00</span> atm</label>
                        <p class="warning-info">★ Mode otomatis: tekanan berubah sesuai PV=nRT</p>
                    </div>
                </div>
                <div class="ctrl-panel">
                    <h3>🌡️ SUHU & PARTIKEL</h3>
                    <div class="slider-group">
                        <label>🔥 Suhu (K) <span id="tempDisplay">350 K</span></label>
                        <input type="range" id="tempSlider" min="100" max="800" value="350" step="5">
                    </div>
                    <div class="slider-group">
                        <label>🧪 Jumlah Partikel <span id="partCountShow">60</span> (max 200)</label>
                        <input type="range" id="partikelSlider" min="10" max="200" value="60" step="5">
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

            <div class="stats-grid">
                <div class="stat-card"><div class="stat-label"><span>🌀</span> TEKANAN GAS</div><div class="stat-value"><span id="pressureActual">1.00</span><span class="stat-unit">atm</span></div><div class="stat-desc">Berdasarkan PV = nRT</div></div>
                <div class="stat-card"><div class="stat-label"><span>📐</span> VOLUME RUANG</div><div class="stat-value"><span id="volumeActual">2.00</span><span class="stat-unit">L</span></div><div class="stat-desc">Liter (L)</div></div>
                <div class="stat-card"><div class="stat-label"><span>⚡</span> KECEPATAN RATA-RATA</div><div class="stat-value"><span id="rmsSpeed">0</span><span class="stat-unit">px/fs</span></div><div class="stat-desc">v_rms partikel</div></div>
                <div class="stat-card"><div class="stat-label"><span>💥</span> ENERGI KINETIK</div><div class="stat-value"><span id="ekStat">0</span><span class="stat-unit">a.u</span></div><div class="stat-desc">½mv² rata-rata</div></div>
                <div class="stat-card"><div class="stat-label"><span>🔁</span> TUMBUKAN / DETIK</div><div class="stat-value"><span id="collisionRate">0</span><span class="stat-unit">Hz</span></div><div class="stat-desc">Frekuensi tumbukan dinding</div></div>
            </div>

            <div class="real-life-note">
                <h4>🌍 <u>SIMULASI INI SEPERTI KEJADIAN NYATA:</u></h4>
                <div style="display: flex; flex-wrap: wrap; gap: 18px; margin-top: 8px;">
                    <div>🚗 Ban Mobil Panas : Suhu ↑ → Tekanan ↑</div>
                    <div>💨 Semprotan Deodoran : Gas memuai cepat</div>
                    <div>🍲 Panci Presto : Volume tetap, suhu & tekanan naik</div>
                    <div>🎈 Balon Udara Panas : Isobarik (tekanan tetap) → volume memuai</div>
                    <div>💉 Jarum Suntik : Volume mengecil → tekanan naik</div>
                </div>
                <div style="margin-top: 10px; background:#ffaa3322; padding:6px; border-radius:30px; text-align:center;">
                    💡 <strong>Mode Isobarik</strong>: Tekanan dijaga tetap, volume otomatis menyesuaikan jika suhu atau jumlah partikel berubah.
                </div>
            </div>
        </div>
    </div>

    <div id="materi" class="page">
        <div class="materi-card">
            <h2 style="color:#ffcd7e;">📘 Termodinamika & Gas Ideal (Kurikulum Merdeka)</h2>
            <div style="display: flex; flex-wrap: wrap; gap: 25px; margin-top: 20px;">
                <div style="flex:1.2;">
                    <h3>✨ Hukum Gas Ideal</h3>
                    <div class="rumus-box"><strong>PV = nRT</strong><br>P = Tekanan (atm), V = Volume (Liter), n = jumlah mol, R = 0.082 L·atm/(mol·K), T = Suhu (Kelvin)</div>
                    <h3>⚙️ Proses Termodinamika</h3>
                    <ul style="margin-left: 1.2rem; line-height: 1.7;">
                        <li><strong>Isobarik</strong> (Tekanan Tetap) → V₁/T₁ = V₂/T₂, V ∝ n</li>
                        <li><strong>Isokhorik</strong> (Volume Tetap) → P₁/T₁ = P₂/T₂</li>
                        <li><strong>Isotermal</strong> (Suhu Tetap) → P₁V₁ = P₂V₂ (Hukum Boyle)</li>
                    </ul>
                </div>
                <div style="flex:1; background:#00000033; border-radius: 1.5rem; padding: 1.2rem;">
                    <h3>🔬 Penerapan Nyata</h3>
                    <p>✔️ Kulkas & AC ✔️ Mesin Kendaraan ✔️ Pernapasan ✔️ Balon Udara Panas (Isobarik)</p>
                    <div class="rumus-box">💡 <strong>Energi Dalam:</strong> ΔU = (3/2)nRΔT (mono) atau (5/2)nRΔT (di)</div>
                </div>
            </div>
        </div>
    </div>

    <div id="lkpd" class="page">
        <div class="lkpd-section-wrapper">
            <div class="tombol-lkpd-nav">
                <button class="btn-lkpd active-lkpd" data-lkpd="lkpd1">📘 LKPD 1 : Hukum Boyle</button>
                <button class="btn-lkpd" data-lkpd="lkpd2">🌡️ LKPD 2 : Hukum Charles</button>
                <button class="btn-lkpd" data-lkpd="lkpd3">⚡ LKPD 3 : Gas Ideal & Energi</button>
            </div>
            <div id="lkpd1" class="lkpd-page active-lkpd-page"><div class="sheet-card"><div class="tujuan-card"><strong>🎯 Tujuan:</strong> Peserta didik mampu mengetahui hubungan antara <strong>Tekanan (P) dan Volume (V)</strong> gas dalam ruang tertutup pada suhu tetap.</div>
                <div class="field-group"><label>❓ Rumusan Masalah</label><textarea id="rm1" rows="2"></textarea></div>
                <div class="field-group"><label>🧪 Hipotesis</label><textarea id="hip1" rows="2"></textarea></div>
                <div class="field-group"><label>📊 Tabel Pengamatan</label><div class="tabel-observasi"><table id="tabel1"><thead><tr><th>No</th><th>Volume (L)</th><th>Tekanan (atm)</th><th>P×V</th></tr></thead><tbody>
                <tr><td>1</td><td class="editable-cell"><input type="text" placeholder="2.0" id="t1_v1"></td><td class="editable-cell"><input type="text" placeholder="..." id="t1_p1"></td><td class="editable-cell"><input type="text" placeholder="..." id="t1_kali1"></td></tr>
                <tr><td>2</td><td class="editable-cell"><input type="text" placeholder="1.5" id="t1_v2"></td><td class="editable-cell"><input type="text" placeholder="..." id="t1_p2"></td><td class="editable-cell"><input type="text" placeholder="..." id="t1_kali2"></td></tr>
                <tr><td>3</td><td class="editable-cell"><input type="text" placeholder="1.0" id="t1_v3"></td><td class="editable-cell"><input type="text" placeholder="..." id="t1_p3"></td><td class="editable-cell"><input type="text" placeholder="..." id="t1_kali3"></td></tr>
                </tbody></table></div></div>
                <div class="field-group"><label>📈 Analisis Data</label><textarea id="analisis1" rows="3"></textarea></div>
                <div class="field-group"><label>💬 Pembahasan</label><textarea id="pembahasan1" rows="3"></textarea></div>
                <div class="field-group"><label>✅ Kesimpulan</label><textarea id="kesimpulan1" rows="2"></textarea></div>
                <button class="btn-simpan-lkpd" data-save="lkpd1">💾 Simpan LKPD 1</button><div class="info-guru" id="statusGuru1">Belum disimpan.</div>
            </div></div>
            <div id="lkpd2" class="lkpd-page"><div class="sheet-card"><div class="tujuan-card"><strong>🎯 Tujuan:</strong> Peserta didik mampu mengetahui hubungan antara <strong>Volume (V) dan Suhu (T)</strong> gas pada tekanan tetap (isobarik).</div>
                <div class="field-group"><label>❓ Rumusan Masalah</label><textarea id="rm2" rows="2"></textarea></div>
                <div class="field-group"><label>🧪 Hipotesis</label><textarea id="hip2" rows="2"></textarea></div>
                <div class="field-group"><label>📊 Tabel Pengamatan</label><div class="tabel-observasi"><table id="tabel2"><thead><tr><th>No</th><th>Suhu (K)</th><th>Volume (L)</th><th>V/T</th></tr></thead><tbody>
                <tr><td>1</td><td class="editable-cell"><input type="text" placeholder="300" id="t2_s1"></td><td class="editable-cell"><input type="text" placeholder="2.0" id="t2_v1"></td><td class="editable-cell"><input type="text" placeholder="..." id="t2_ratio1"></td></tr>
                <tr><td>2</td><td class="editable-cell"><input type="text" placeholder="450" id="t2_s2"></td><td class="editable-cell"><input type="text" placeholder="..." id="t2_v2"></td><td class="editable-cell"><input type="text" placeholder="..." id="t2_ratio2"></td></tr>
                <tr><td>3</td><td class="editable-cell"><input type="text" placeholder="600" id="t2_s3"></td><td class="editable-cell"><input type="text" placeholder="..." id="t2_v3"></td><td class="editable-cell"><input type="text" placeholder="..." id="t2_ratio3"></td></tr>
                </tbody></table></div></div>
                <div class="field-group"><label>📈 Analisis Data</label><textarea id="analisis2" rows="3"></textarea></div>
                <div class="field-group"><label>💬 Pembahasan</label><textarea id="pembahasan2" rows="3"></textarea></div>
                <div class="field-group"><label>✅ Kesimpulan</label><textarea id="kesimpulan2" rows="2"></textarea></div>
                <button class="btn-simpan-lkpd" data-save="lkpd2">💾 Simpan LKPD 2</button><div class="info-guru" id="statusGuru2">Belum disimpan.</div>
            </div></div>
            <div id="lkpd3" class="lkpd-page"><div class="sheet-card"><div class="tujuan-card"><strong>🎯 Tujuan:</strong> Peserta didik mampu mengetahui hubungan antara Tekanan (P), Volume (V), Suhu (T), dan jumlah partikel (n).</div>
                <div class="field-group"><label>❓ Rumusan Masalah</label><textarea id="rm3" rows="2"></textarea></div>
                <div class="field-group"><label>🧪 Hipotesis</label><textarea id="hip3" rows="2"></textarea></div>
                <div class="field-group"><label>📊 Tabel Pengamatan</label><div class="tabel-observasi"><table id="tabel3"><thead><tr><th>Eksp</th><th>Jml Partikel</th><th>Suhu (K)</th><th>Tekanan (atm)</th><th>Energi Kinetik</th><th>PV/(nT)</th></tr></thead><tbody>
                <tr><td>1</td><td class="editable-cell"><input type="text" placeholder="40" id="t3_n1"></td><td class="editable-cell"><input type="text" placeholder="300" id="t3_t1"></td><td class="editable-cell"><input type="text" placeholder="..." id="t3_p1"></td><td class="editable-cell"><input type="text" placeholder="..." id="t3_e1"></td><td class="editable-cell"><input type="text" placeholder="..." id="t3_const1"></td></tr>
                <tr><td>2</td><td class="editable-cell"><input type="text" placeholder="80" id="t3_n2"></td><td class="editable-cell"><input type="text" placeholder="450" id="t3_t2"></td><td class="editable-cell"><input type="text" placeholder="..." id="t3_p2"></td><td class="editable-cell"><input type="text" placeholder="..." id="t3_e2"></td><td class="editable-cell"><input type="text" placeholder="..." id="t3_const2"></td></tr>
                <tr><td>3</td><td class="editable-cell"><input type="text" placeholder="120" id="t3_n3"></td><td class="editable-cell"><input type="text" placeholder="600" id="t3_t3"></td><td class="editable-cell"><input type="text" placeholder="..." id="t3_p3"></td><td class="editable-cell"><input type="text" placeholder="..." id="t3_e3"></td><td class="editable-cell"><input type="text" placeholder="..." id="t3_const3"></td></tr>
                </tbody></table></div></div>
                <div class="field-group"><label>📈 Analisis Data</label><textarea id="analisis3" rows="3"></textarea></div>
                <div class="field-group"><label>💬 Pembahasan</label><textarea id="pembahasan3" rows="3"></textarea></div>
                <div class="field-group"><label>✅ Kesimpulan</label><textarea id="kesimpulan3" rows="2"></textarea></div>
                <div class="flex-buttons"><button class="btn-simpan-lkpd" data-save="lkpd3">💾 Simpan LKPD 3</button><button id="exportAllGuru" class="btn-simpan-lkpd btn-ekspor-semua">📎 Ekspor Semua Jawaban</button></div>
                <div class="info-guru" id="statusGuru3">Data tersimpan lokal.</div>
            </div></div>
        </div>
    </div>
</div>

<script>
    // ======================== SIMULASI GAS dengan Mode Isobarik ========================
    const canvas = document.getElementById('gasCanvas');
    const ctx = canvas.getContext('2d');
    let width = 1150, height = 520;
    canvas.width = width; canvas.height = height;
    let particles = [];
    
    let temperature = 350;
    let volumeLiter = 2.0;
    let particleCount = 60;
    let particleType = 'mono';
    let elementName = 'Helium';
    let massFactor = 1.0;
    let rightWallX = width * ((volumeLiter - 0.5) / 3.5);
    let collisionCounter = 0;
    let lastCollisionUpdate = Date.now();
    let collisionRate = 0;
    const baseSpeedRef = 7.5;
    
    // Mode: 'auto' atau 'isobaric'
    let currentMode = 'auto';  // auto: tekanan berubah, isobaric: tekanan tetap
    let fixedPressure = 1.0;   // tekanan yang dijaga tetap pada mode isobarik
    
    function calculatePressureFromParams() {
        let pressure = (particleCount / 60) * (temperature / 350) * (2.0 / volumeLiter);
        pressure = pressure * (massFactor > 1.5 ? 1.1 : 1.0);
        return Math.min(6.0, Math.max(0.1, pressure));
    }
    
    // Pada mode isobarik, volume harus menyesuaikan agar tekanan = fixedPressure
    function updateVolumeForIsobaric() {
        // Rumus: P_target = (n/60)*(T/350)*(2/V) * faktor
        // Maka V = (n/60)*(T/350)*2 / P_target * faktor
        let targetVol = (particleCount / 60) * (temperature / 350) * 2.0 / fixedPressure;
        targetVol = targetVol / (massFactor > 1.5 ? 1.1 : 1.0);
        targetVol = Math.min(4.0, Math.max(0.5, targetVol));
        if (Math.abs(volumeLiter - targetVol) > 0.02) {
            volumeLiter = targetVol;
            updateVolumeDisplay();
            adjustParticlePositionsForVolume();
        }
    }
    
    function updateVolumeDisplay() {
        document.getElementById('volumeActual').innerText = volumeLiter.toFixed(2);
        document.getElementById('volumeValueLabel').innerHTML = volumeLiter.toFixed(2) + " L";
        let percent = (volumeLiter - 0.5) / 3.5;
        rightWallX = width * Math.min(1.0, Math.max(0.0, percent));
        rightWallX = Math.min(width - 10, Math.max(70, rightWallX));
    }
    
    function adjustParticlePositionsForVolume() {
        let maxX = rightWallX;
        for (let p of particles) {
            if (p.x + p.r > maxX) p.x = maxX - p.r;
            if (p.x - p.r < 0) p.x = p.r;
        }
    }
    
    function getTargetSpeed() {
        return baseSpeedRef * Math.sqrt(temperature / 300) / Math.sqrt(massFactor);
    }
    
    function applyTemperatureToParticles() {
        let targetSpeed = getTargetSpeed();
        for (let p of particles) {
            let currentSpeed = Math.hypot(p.vx, p.vy);
            if (currentSpeed < 0.5) {
                let angle = Math.random() * 2 * Math.PI;
                p.vx = Math.cos(angle) * targetSpeed * (0.7 + Math.random() * 0.8);
                p.vy = Math.sin(angle) * targetSpeed * (0.7 + Math.random() * 0.8);
            } else {
                let ratio = targetSpeed / currentSpeed;
                ratio = Math.min(2.0, Math.max(0.5, ratio));
                p.vx *= ratio;
                p.vy *= ratio;
            }
        }
    }
    
    function updateMassFactor() {
        if (particleType === 'mono') {
            if (elementName === 'Helium') massFactor = 1.0;
            else if (elementName === 'Neon') massFactor = 1.9;
            else massFactor = 1.2;
        } else {
            if (elementName === 'Oksigen') massFactor = 2.66;
            else if (elementName === 'Nitrogen') massFactor = 2.33;
            else massFactor = 2.0;
        }
        applyTemperatureToParticles();
    }
    
    function initParticles(count) {
        let arr = [];
        let targetSpeed = getTargetSpeed();
        let maxX = rightWallX;
        for (let i = 0; i < count; i++) {
            let x = Math.random() * (maxX - 14) + 7;
            let y = Math.random() * (height - 14) + 7;
            let angle = Math.random() * 2 * Math.PI;
            let speed = targetSpeed * (0.6 + Math.random() * 0.8);
            arr.push({x, y, vx: Math.cos(angle) * speed, vy: Math.sin(angle) * speed, r: 4.5});
        }
        return arr;
    }
    
    function setParticleCount(newCount) {
        newCount = Math.min(200, Math.max(10, newCount));
        let cur = particles.length;
        let targetSpeed = getTargetSpeed();
        let maxX = rightWallX;
        if (newCount > cur) {
            for (let i = 0; i < newCount - cur; i++) {
                let x = Math.random() * (maxX - 14) + 7;
                let y = Math.random() * (height - 14) + 7;
                let angle = Math.random() * 2 * Math.PI;
                let speed = targetSpeed * (0.6 + Math.random() * 0.8);
                particles.push({x, y, vx: Math.cos(angle) * speed, vy: Math.sin(angle) * speed, r: 4.5});
            }
        } else if (newCount < cur) {
            particles.splice(newCount, cur - newCount);
        }
        particleCount = particles.length;
        document.getElementById('partCountShow').innerText = particleCount;
        
        if (currentMode === 'isobaric') {
            updateVolumeForIsobaric();
        }
        updateStats();
    }
    
    function handleCollisions() {
        let curR = rightWallX;
        for (let p of particles) {
            p.x += p.vx; p.y += p.vy;
            if (p.x - p.r < 0) { p.x = p.r; p.vx = -p.vx; collisionCounter++; }
            if (p.x + p.r > curR) { p.x = curR - p.r; p.vx = -p.vx; collisionCounter++; }
            if (p.y - p.r < 0) { p.y = p.r; p.vy = -p.vy; collisionCounter++; }
            if (p.y + p.r > height) { p.y = height - p.r; p.vy = -p.vy; collisionCounter++; }
        }
        for (let i = 0; i < particles.length; i++) {
            for (let j = i + 1; j < particles.length; j++) {
                let p1 = particles[i], p2 = particles[j];
                let dx = p2.x - p1.x, dy = p2.y - p1.y;
                let dist = Math.hypot(dx, dy);
                let minD = p1.r + p2.r;
                if (dist < minD) {
                    let nx = dx / dist, ny = dy / dist;
                    let vrelx = p2.vx - p1.vx, vrely = p2.vy - p1.vy;
                    let velAlong = vrelx * nx + vrely * ny;
                    if (velAlong < 0) {
                        p1.vx += velAlong * nx; p1.vy += velAlong * ny;
                        p2.vx -= velAlong * nx; p2.vy -= velAlong * ny;
                        collisionCounter++;
                    }
                    let overlap = minD - dist;
                    let cx = nx * overlap * 0.5, cy = ny * overlap * 0.5;
                    p1.x -= cx; p1.y -= cy;
                    p2.x += cx; p2.y += cy;
                }
            }
        }
    }
    
    function updateStats() {
        let totalSpeed = 0;
        for (let p of particles) totalSpeed += Math.hypot(p.vx, p.vy);
        let avgSpeed = particles.length ? totalSpeed / particles.length : 0;
        document.getElementById('rmsSpeed').innerText = avgSpeed.toFixed(1);
        let avgEk = 0.5 * massFactor * avgSpeed * avgSpeed;
        document.getElementById('ekStat').innerText = avgEk.toFixed(1);
        
        let pressure = calculatePressureFromParams();
        document.getElementById('pressureActual').innerHTML = pressure.toFixed(2);
        document.getElementById('pressureDisplay').innerHTML = pressure.toFixed(2);
        document.getElementById('tempDisplay').innerText = Math.round(temperature) + " K";
        
        let now = Date.now();
        let dt = (now - lastCollisionUpdate) / 1000;
        if (dt >= 0.8) { collisionRate = Math.round(collisionCounter / dt); collisionCounter = 0; lastCollisionUpdate = now; }
        document.getElementById('collisionRate').innerHTML = collisionRate;
    }
    
    function draw() {
        ctx.clearRect(0, 0, width, height);
        ctx.strokeStyle = "#ffcc77"; ctx.lineWidth = 3;
        ctx.strokeRect(5, 5, rightWallX - 10, height - 10);
        ctx.fillStyle = "#bd7f3ad9"; ctx.fillRect(rightWallX - 12, 0, 14, height);
        ctx.fillStyle = "#f3bc6c"; ctx.fillRect(rightWallX - 10, 0, 10, height);
        for (let p of particles) {
            let grad = ctx.createRadialGradient(p.x - 3, p.y - 3, 2, p.x, p.y, p.r + 3);
            if (temperature > 500) grad.addColorStop(0, '#ffaa66');
            else if (temperature < 200) grad.addColorStop(0, '#88ccff');
            else grad.addColorStop(0, '#ffdd99');
            grad.addColorStop(1, '#e0872c');
            ctx.beginPath(); ctx.arc(p.x, p.y, p.r, 0, Math.PI * 2);
            ctx.fillStyle = grad; ctx.fill();
        }
        updateStats();
    }
    
    function animate() { handleCollisions(); draw(); requestAnimationFrame(animate); }
    
    particles = initParticles(60);
    particleCount = 60;
    
    // Event Listeners
    document.getElementById('tempSlider').addEventListener('input', (e) => { 
        temperature = parseInt(e.target.value); 
        applyTemperatureToParticles(); 
        if (currentMode === 'isobaric') updateVolumeForIsobaric();
        updateStats(); 
    });
    document.getElementById('partikelSlider').addEventListener('input', (e) => { 
        setParticleCount(parseInt(e.target.value)); 
    });
    document.getElementById('volumeSlider').addEventListener('input', (e) => { 
        if (currentMode === 'auto') {
            volumeLiter = parseFloat(e.target.value);
            updateVolumeDisplay();
            adjustParticlePositionsForVolume();
            updateStats();
        }
    });
    document.getElementById('particleTypeSelect').onchange = (e) => { 
        particleType = e.target.value; 
        updateMassFactor(); 
        if (currentMode === 'isobaric') updateVolumeForIsobaric();
        updateStats(); 
    };
    document.getElementById('elementSelect').onchange = (e) => { 
        elementName = e.target.value; 
        updateMassFactor(); 
        if (currentMode === 'isobaric') updateVolumeForIsobaric();
        updateStats(); 
    };
    document.getElementById('fixedPressureSlider').addEventListener('input', (e) => {
        fixedPressure = parseFloat(e.target.value);
        document.getElementById('fixedPressureLabel').innerText = fixedPressure.toFixed(2);
        if (currentMode === 'isobaric') {
            updateVolumeForIsobaric();
            updateStats();
        }
    });
    
    // Mode switching
    const modeAutoBtn = document.getElementById('modeAutoBtn');
    const modeIsobaricBtn = document.getElementById('ModeIsobarikBtn');
    const fixedSlider = document.getElementById('fixedPressureSlider');
    const volumeSlider = document.getElementById('volumeSlider');
    
    modeAutoBtn.addEventListener('click', () => {
        currentMode = 'auto';
        modeAutoBtn.classList.add('active-mode');
        modeIsobaricBtn.classList.remove('active-mode');
        fixedSlider.disabled = true;
        volumeSlider.disabled = false;
        updateStats();
    });
    modeIsobaricBtn.addEventListener('click', () => {
        currentMode = 'isobaric';
        modeAutoBtn.classList.remove('active-mode');
        modeIsobaricBtn.classList.add('active-mode');
        fixedSlider.disabled = false;
        volumeSlider.disabled = true;
        updateVolumeForIsobaric();
        updateStats();
    });
    
    document.getElementById('resetSimBtn').onclick = () => {
        temperature = 350;
        volumeLiter = 2.0;
        particleType = 'mono';
        elementName = 'Helium';
        fixedPressure = 1.0;
        currentMode = 'auto';
        document.getElementById('tempSlider').value = 350;
        document.getElementById('volumeSlider').value = 2.0;
        document.getElementById('partikelSlider').value = 60;
        document.getElementById('particleTypeSelect').value = 'mono';
        document.getElementById('elementSelect').value = 'Helium';
        document.getElementById('fixedPressureSlider').value = 1.0;
        document.getElementById('fixedPressureLabel').innerText = "1.00";
        modeAutoBtn.classList.add('active-mode');
        modeIsobaricBtn.classList.remove('active-mode');
        fixedSlider.disabled = true;
        volumeSlider.disabled = false;
        updateMassFactor();
        updateVolumeDisplay();
        adjustParticlePositionsForVolume();
        setParticleCount(60);
        applyTemperatureToParticles();
        updateStats();
    };
    
    animate();
    
    // LKPD Save/Load (sama seperti sebelumnya, disingkat)
    function saveLkpd(lkpdId){
        if(lkpdId === 'lkpd1'){
            const data = { rumusan: document.getElementById('rm1').value, hipotesis: document.getElementById('hip1').value, tabel: { v1: document.getElementById('t1_v1')?.value, p1: document.getElementById('t1_p1')?.value, kali1: document.getElementById('t1_kali1')?.value, v2: document.getElementById('t1_v2')?.value, p2: document.getElementById('t1_p2')?.value, kali2: document.getElementById('t1_kali2')?.value, v3: document.getElementById('t1_v3')?.value, p3: document.getElementById('t1_p3')?.value, kali3: document.getElementById('t1_kali3')?.value }, analisis: document.getElementById('analisis1').value, pembahasan: document.getElementById('pembahasan1').value, kesimpulan: document.getElementById('kesimpulan1').value, timestamp: new Date().toISOString() };
            localStorage.setItem('LKPD_GAS_1', JSON.stringify(data));
            document.getElementById('statusGuru1').innerHTML = `✅ Tersimpan (${new Date().toLocaleTimeString()})`;
        } else if(lkpdId === 'lkpd2'){
            const data = { rumusan: document.getElementById('rm2').value, hipotesis: document.getElementById('hip2').value, tabel: { s1: document.getElementById('t2_s1')?.value, v1: document.getElementById('t2_v1')?.value, ratio1: document.getElementById('t2_ratio1')?.value, s2: document.getElementById('t2_s2')?.value, v2: document.getElementById('t2_v2')?.value, ratio2: document.getElementById('t2_ratio2')?.value, s3: document.getElementById('t2_s3')?.value, v3: document.getElementById('t2_v3')?.value, ratio3: document.getElementById('t2_ratio3')?.value }, analisis: document.getElementById('analisis2').value, pembahasan: document.getElementById('pembahasan2').value, kesimpulan: document.getElementById('kesimpulan2').value, timestamp: new Date().toISOString() };
            localStorage.setItem('LKPD_GAS_2', JSON.stringify(data));
            document.getElementById('statusGuru2').innerHTML = `✅ Tersimpan (${new Date().toLocaleTimeString()})`;
        } else if(lkpdId === 'lkpd3'){
            const data = { rumusan: document.getElementById('rm3').value, hipotesis: document.getElementById('hip3').value, tabel: { n1: document.getElementById('t3_n1')?.value, t1: document.getElementById('t3_t1')?.value, p1: document.getElementById('t3_p1')?.value, e1: document.getElementById('t3_e1')?.value, const1: document.getElementById('t3_const1')?.value, n2: document.getElementById('t3_n2')?.value, t2: document.getElementById('t3_t2')?.value, p2: document.getElementById('t3_p2')?.value, e2: document.getElementById('t3_e2')?.value, const2: document.getElementById('t3_const2')?.value, n3: document.getElementById('t3_n3')?.value, t3: document.getElementById('t3_t3')?.value, p3: document.getElementById('t3_p3')?.value, e3: document.getElementById('t3_e3')?.value, const3: document.getElementById('t3_const3')?.value }, analisis: document.getElementById('analisis3').value, pembahasan: document.getElementById('pembahasan3').value, kesimpulan: document.getElementById('kesimpulan3').value, timestamp: new Date().toISOString() };
            localStorage.setItem('LKPD_GAS_3', JSON.stringify(data));
            document.getElementById('statusGuru3').innerHTML = `✅ Tersimpan (${new Date().toLocaleTimeString()})`;
        }
    }
    function loadAllSaved(){
        let s1=localStorage.getItem('LKPD_GAS_1'); if(s1){ let d=JSON.parse(s1); document.getElementById('rm1').value=d.rumusan||''; document.getElementById('hip1').value=d.hipotesis||''; if(d.tabel){ for(let k in d.tabel) if(document.getElementById(k)) document.getElementById(k).value=d.tabel[k]; } document.getElementById('analisis1').value=d.analisis||''; document.getElementById('pembahasan1').value=d.pembahasan||''; document.getElementById('kesimpulan1').value=d.kesimpulan||''; document.getElementById('statusGuru1').innerHTML=`🔄 Dimuat (${d.timestamp?.slice(0,19)})`; }
        let s2=localStorage.getItem('LKPD_GAS_2'); if(s2){ let d=JSON.parse(s2); document.getElementById('rm2').value=d.rumusan||''; document.getElementById('hip2').value=d.hipotesis||''; if(d.tabel){ for(let k in d.tabel) if(document.getElementById(k)) document.getElementById(k).value=d.tabel[k]; } document.getElementById('analisis2').value=d.analisis||''; document.getElementById('pembahasan2').value=d.pembahasan||''; document.getElementById('kesimpulan2').value=d.kesimpulan||''; document.getElementById('statusGuru2').innerHTML=`🔄 Dimuat`; }
        let s3=localStorage.getItem('LKPD_GAS_3'); if(s3){ let d=JSON.parse(s3); document.getElementById('rm3').value=d.rumusan||''; document.getElementById('hip3').value=d.hipotesis||''; if(d.tabel){ for(let k in d.tabel) if(document.getElementById(k)) document.getElementById(k).value=d.tabel[k]; } document.getElementById('analisis3').value=d.analisis||''; document.getElementById('pembahasan3').value=d.pembahasan||''; document.getElementById('kesimpulan3').value=d.kesimpulan||''; document.getElementById('statusGuru3').innerHTML=`🔄 Dimuat`; }
    }
    function exportAllGuru(){
        let output = "=== LAPORAN JAWABAN LKPD SISWA ===\nWaktu: "+new Date().toLocaleString()+"\n\n"; 
        ['LKPD_GAS_1','LKPD_GAS_2','LKPD_GAS_3'].forEach(k=>{ let raw=localStorage.getItem(k); if(raw) output+=`\n--- ${k} ---\n${raw}\n`; else output+=`\n--- ${k} : kosong ---\n`; });
        const blob = new Blob([output], {type:'text/plain'}); const a=document.createElement('a'); a.href=URL.createObjectURL(blob); a.download=`Laporan_LKPD_Siswa_${Date.now()}.txt`; a.click(); URL.revokeObjectURL(a.href);
    }
    document.querySelectorAll('.btn-simpan-lkpd').forEach(btn=>btn.addEventListener('click',function(){ saveLkpd(this.getAttribute('data-save')); }));
    document.getElementById('exportAllGuru')?.addEventListener('click',exportAllGuru);
    loadAllSaved();
    
    document.querySelectorAll('.nav-btn').forEach(btn=>{ btn.addEventListener('click',()=>{ document.querySelectorAll('.nav-btn').forEach(b=>b.classList.remove('active')); btn.classList.add('active'); document.querySelectorAll('.page').forEach(p=>p.classList.remove('active-page')); document.getElementById(btn.getAttribute('data-page')).classList.add('active-page'); }); });
    document.querySelectorAll('.btn-lkpd').forEach(btn=>{ btn.addEventListener('click',()=>{ document.querySelectorAll('.btn-lkpd').forEach(b=>b.classList.remove('active-lkpd')); btn.classList.add('active-lkpd'); document.querySelectorAll('.lkpd-page').forEach(p=>p.classList.remove('active-lkpd-page')); document.getElementById(btn.getAttribute('data-lkpd')).classList.add('active-lkpd-page'); }); });
</script>
</body>
</html>
