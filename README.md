<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>🔬 Laboratorium Virtual Termodinamika - Gas Ideal & Proses Termodinamika</title>
    <style>
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
        }
        
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #0f0f23 0%, #1a1a2e 50%, #16213e 100%);
            color: #e0e6ed;
            overflow-x: hidden;
            min-height: 100vh;
        }

        /* Navigation */
        .navbar {
            position: fixed;
            top: 0;
            width: 100%;
            background: rgba(15, 15, 35, 0.95);
            backdrop-filter: blur(20px);
            padding: 1rem 2rem;
            z-index: 1000;
            border-bottom: 1px solid rgba(255,255,255,0.1);
        }
        
        .nav-container {
            max-width: 1400px;
            margin: 0 auto;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        
        .logo {
            font-size: 1.8rem;
            font-weight: 700;
            background: linear-gradient(45deg, #ffd700, #ff6b35);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
        }
        
        .progress-bar {
            flex: 1;
            margin: 0 2rem;
            height: 4px;
            background: rgba(255,255,255,0.1);
            border-radius: 2px;
            overflow: hidden;
        }
        
        .progress-fill {
            height: 100%;
            background: linear-gradient(90deg, #4facfe 0%, #00f2fe 100%);
            width: 0%;
            transition: width 0.5s ease;
            border-radius: 2px;
        }

        /* Page Container */
        .page-container {
            min-height: 100vh;
            padding-top: 80px;
            opacity: 0;
            transform: translateY(30px);
            transition: all 0.8s cubic-bezier(0.25, 0.46, 0.45, 0.94);
        }
        
        .page-container.active {
            opacity: 1;
            transform: translateY(0);
        }

        /* Page 1: Landing */
        .landing {
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            text-align: center;
            padding: 2rem;
            max-width: 1400px;
            margin: 0 auto;
        }
        
        .hero-title {
            font-size: clamp(2.5rem, 5vw, 4rem);
            margin-bottom: 1rem;
            background: linear-gradient(45deg, #ffd700, #ff6b35, #4facfe);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
            text-shadow: 0 0 30px rgba(255,215,0,0.3);
        }
        
        .hero-subtitle {
            font-size: 1.3rem;
            margin-bottom: 3rem;
            color: #b0b3c1;
            max-width: 600px;
        }
        
        .cta-button {
            background: linear-gradient(45deg, #ffd700, #ff6b35);
            color: #1a1a2e;
            border: none;
            padding: 1.2rem 3rem;
            font-size: 1.2rem;
            font-weight: 700;
            border-radius: 50px;
            cursor: pointer;
            box-shadow: 0 10px 30px rgba(255,215,0,0.4);
            transition: all 0.3s ease;
            text-transform: uppercase;
            letter-spacing: 1px;
        }
        
        .cta-button:hover {
            transform: translateY(-3px);
            box-shadow: 0 15px 40px rgba(255,215,0,0.6);
        }

        /* Page 2: Materi */
        .materi-page {
            max-width: 1400px;
            margin: 0 auto;
            padding: 3rem 2rem;
        }
        
        .section-title {
            font-size: 2.5rem;
            text-align: center;
            margin-bottom: 3rem;
            background: linear-gradient(45deg, #4facfe, #00f2fe);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
        }
        
        .materi-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(400px, 1fr));
            gap: 2rem;
            margin-bottom: 3rem;
        }
        
        .materi-card {
            background: rgba(255,255,255,0.05);
            backdrop-filter: blur(15px);
            border-radius: 20px;
            padding: 2rem;
            border: 1px solid rgba(255,255,255,0.1);
            transition: all 0.3s ease;
            position: relative;
            overflow: hidden;
        }
        
        .materi-card::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            height: 4px;
            background: linear-gradient(90deg, #ffd700, #ff6b35);
        }
        
        .materi-card:hover {
            transform: translateY(-10px);
            box-shadow: 0 20px 40px rgba(0,0,0,0.3);
        }
        
        .materi-icon {
            font-size: 3rem;
            margin-bottom: 1rem;
            display: block;
        }
        
        .materi-card h3 {
            font-size: 1.5rem;
            margin-bottom: 1rem;
            color: #ffd700;
        }

        /* Page 3: Simulasi */
        .simulasi-container {
            max-width: 1400px;
            margin: 0 auto;
            padding: 2rem;
        }
        
        .simulasi-header {
            text-align: center;
            margin-bottom: 2rem;
        }
        
        .process-selector {
            display: flex;
            gap: 1rem;
            justify-content: center;
            margin-bottom: 2rem;
            flex-wrap: wrap;
        }
        
        .process-btn {
            background: rgba(255,255,255,0.1);
            border: 2px solid #4facfe;
            color: #e0e6ed;
            padding: 0.8rem 1.5rem;
            border-radius: 25px;
            cursor: pointer;
            transition: all 0.3s ease;
            font-weight: 600;
        }
        
        .process-btn.active {
            background: linear-gradient(45deg, #4facfe, #00f2fe);
            color: #1a1a2e;
            box-shadow: 0 5px 15px rgba(79,172,254,0.4);
        }
        
        /* Canvas container sama seperti sebelumnya */
        .canvas-container {
            background: #0b1421;
            border-radius: 32px;
            padding: 10px;
            box-shadow: inset 0 -3px 8px #00000099, 0 12px 25px black;
            border: 1px solid #6a7ca0;
            margin-bottom: 2rem;
        }

        /* LKPD Page */
        .lkpd-container {
            max-width: 1000px;
            margin: 0 auto;
            padding: 3rem 2rem;
        }
        
        .lkpd-form {
            background: rgba(255,255,255,0.05);
            backdrop-filter: blur(15px);
            border-radius: 20px;
            padding: 2.5rem;
            border: 1px solid rgba(255,255,255,0.1);
        }
        
        .form-group {
            margin-bottom: 1.5rem;
        }
        
        .form-group label {
            display: block;
            margin-bottom: 0.5rem;
            font-weight: 600;
            color: #ffd700;
        }
        
        .form-group input, .form-group textarea, .form-group select {
            width: 100%;
            padding: 0.8rem;
            border: 2px solid rgba(255,255,255,0.2);
            border-radius: 10px;
            background: rgba(255,255,255,0.05);
            color: #e0e6ed;
            font-size: 1rem;
            transition: border-color 0.3s ease;
        }
        
        .form-group input:focus, .form-group textarea:focus, .form-group select:focus {
            outline: none;
            border-color: #4facfe;
            box-shadow: 0 0 10px rgba(79,172,254,0.3);
        }
        
        .submit-btn {
            width: 100%;
            background: linear-gradient(45deg, #4facfe, #00f2fe);
            color: #1a1a2e;
            border: none;
            padding: 1rem;
            font-size: 1.2rem;
            font-weight: 700;
            border-radius: 15px;
            cursor: pointer;
            transition: all 0.3s ease;
        }
        
        .submit-btn:hover:not(:disabled) {
            transform: translateY(-2px);
            box-shadow: 0 10px 25px rgba(79,172,254,0.4);
        }
        
        .submit-btn:disabled {
            opacity: 0.5;
            cursor: not-allowed;
        }

        /* Real-time monitoring */
        .monitoring-panel {
            background: rgba(255,255,255,0.03);
            border-radius: 15px;
            padding: 1.5rem;
            margin-top: 2rem;
            border: 1px solid rgba(255,215,0,0.2);
        }
        
        .monitoring-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 1rem;
        }
        
        .monitor-item {
            text-align: center;
            padding: 1rem;
            background: rgba(255,255,255,0.05);
            border-radius: 10px;
        }
        
        .monitor-value {
            font-size: 2rem;
            font-weight: 700;
            color: #ffd700;
        }

        @media (max-width: 768px) {
            .materi-grid {
                grid-template-columns: 1fr;
            }
            .process-selector {
                flex-direction: column;
                align-items: center;
            }
        }
    </style>
</head>
<body>
    <!-- Navigation -->
    <nav class="navbar">
        <div class="nav-container">
            <div class="logo">🔬 Lab Virtual Termodinamika</div>
            <div class="progress-bar">
                <div class="progress-fill" id="progressFill"></div>
            </div>
            <div>Halaman <span id="currentPage">1</span>/4</div>
        </div>
    </nav>

    <!-- Page 1: Landing -->
    <div id="page1" class="page-container active">
        <div class="landing">
            <h1 class="hero-title">Laboratorium Virtual Termodinamika</h1>
            <p class="hero-subtitle">
                Pelajari Gas Ideal, Proses Isochoric, Isobaric, Isothermal dengan simulasi interaktif 
                yang lebih canggih dari PhET. Lengkap dengan LKPD online real-time!
            </p>
            <button class="cta-button" onclick="nextPage()">🚀 Mulai Belajar</button>
        </div>
    </div>

    <!-- Page 2: Materi -->
    <div id="page2" class="page-container materi-page">
        <h2 class="section-title">📚 Materi Pembelajaran</h2>
        <div class="materi-grid">
            <div class="materi-card">
                <span class="materi-icon">⚛️</span>
                <h3>Gas Ideal</h3>
                <p><strong>Persamaan Gas Ideal:</strong> PV = nRT</p>
                <ul style="margin-top: 1rem;">
                    <li>P → Tekanan (Pa)</li>
                    <li>V → Volume (m³)</li>
                    <li>n → Jumlah mol</li>
                    <li>R → 8.314 J/mol·K</li>
                    <li>T → Suhu (K)</li>
                </ul>
            </div>
            <div class="materi-card">
                <span class="materi-icon">🔄</span>
                <h3>Proses Isothermal</h3>
                <p>Suhu konstan (ΔT = 0)</p>
                <ul style="margin-top: 1rem;">
                    <li>PV = konstan</li>
                    <li>W = nRT ln(V₂/V₁)</li>
                    <li>ΔU = 0</li>
                    <li>Q = W</li>
                </ul>
            </div>
            <div class="materi-card">
                <span class="materi-icon">📏</span>
                <h3>Proses Isobaric</h3>
                <p>Tekanan konstan (ΔP = 0)</p>
                <ul style="margin-top: 1rem;">
                    <li>V/T = konstan</li>
                    <li>W = PΔV</li>
                    <li>ΔU = nCᵥΔT</li>
                    <li>Q = nCₚΔT</li>
                </ul>
            </div>
            <div class="materi-card">
                <span class="materi-icon">📦</span>
                <h3>Proses Isochoric</h3>
                <p>Volume konstan (ΔV = 0)</p>
                <ul style="margin-top: 1rem;">
                    <li>P/T = konstan</li>
                    <li>W = 0</li>
                    <li>ΔU = Q = nCᵥΔT</li>
                </ul>
            </div>
        </div>
        <div style="text-align: center;">
            <button class="cta-button" onclick="nextPage()" style="margin: 0 1rem;">➡️ Ke Simulasi</button>
            <button class="cta-button" onclick="prevPage()" style="background: rgba(255,255,255,0.1); color: white;">⬅️ Kembali</button>
        </div>
    </div>

    <!-- Page 3: Simulasi -->
    <div id="page3" class="page-container simulasi-container">
        <div class="simulasi-header">
            <h2 class="section-title">🎮 Simulasi Interaktif</h2>
            <p>Pilih proses termodinamika yang ingin disimulasikan:</p>
            <div class="process-selector">
                <button class="process-btn active" data-process="free">Gerak Bebas</button>
                <button class="process-btn" data-process="isothermal">Isothermal</button>
                <button class="process-btn" data-process="isobaric">Isobaric</button>
                <button class="process-btn" data-process="isochoric">Isochoric</button>
            </div>
        </div>

        <!-- Canvas simulasi (kode asli yang diupgrade) -->
        <div class="canvas-container">
            <canvas id="gasCanvas" width="1200" height="600"></canvas>
        </div>

        <div style="display: flex; gap: 1rem; justify-content: center; flex-wrap: wrap;">
            <button class="cta-button" onclick="nextPage()" style="padding: 0.8rem 2rem; font-size: 1rem;">📝 LKPD</button>
            <button class="cta-button" onclick="prevPage()" style="background: rgba(255,255,255,0.1); color: white; padding: 0.8rem 2rem; font-size: 1rem;">⬅️ Kembali</button>
        </div>
    </div>

    <!-- Page 4: LKPD -->
    <div id="page4" class="page-container">
        <div class="lkpd-container">
            <h2 class="section-title" style="text-align: center; margin-bottom: 2rem;">📝 Lembar Kerja Praktikum Digital</h2>
            
            <form id="lkpdForm" class="lkpd-form">
                <div class="form-group">
                    <label>Nama Siswa *</label>
                    <input type="text" id="nama" required>
                </div>
                
                <div class="form-group">
                    <label>Kelas *</label>
                    <select id="kelas" required>
                        <option value="">Pilih Kelas</option>
                        <option value="X MIPA 1">X MIPA 1</option>
                        <option value="X MIPA 2">X MIPA 2</option>
                        <option value="X MIPA 3">X MIPA 3</option>
                        <option value="XI MIPA 1">XI MIPA 1</option>
                        <option value="XI MIPA 2">XI MIPA 2</option>
                        <option value="XII MIPA 1">XII MIPA 1</option>
                        <option value="XII MIPA 2">XII MIPA 2</option>
                    </select>
                </div>

                <h3 style="color: #ffd700; margin: 2rem 0 1rem 0;">Observasi Simulasi</h3>
                
                <div class="
