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
            flex
