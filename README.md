<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Labirin Hijaiyah Interaktif - PAI Kelas 1</title>
    <style>
        :root {
            --primary-color: #2E7D32;
            --secondary-color: #FFD54F;
            --bg-color: #E3F2FD;
            --text-color: #1B5E20;
            --wall-color: #A5D6A7;
            --panel-bg: rgba(255, 255, 255, 0.95);
        }

        body {
            margin: 0;
            padding: 0;
            font-family: 'Segoe UI', Roboto, Helvetica, Arial, sans-serif;
            background-color: var(--bg-color);
            color: var(--text-color);
            height: 100vh;
            width: 100vw;
            display: flex;
            justify-content: center;
            align-items: center;
            overflow: hidden;
            touch-action: none;
        }

        #game-wrapper {
            width: 95vw;
            height: 90vh;
            display: flex;
            gap: 20px;
            background: var(--panel-bg);
            padding: 20px;
            border-radius: 30px;
            box-shadow: 0 25px 60px rgba(0,0,0,0.2);
            border: 10px solid white;
            visibility: hidden;
        }

        .sidebar {
            width: 280px;
            display: flex;
            flex-direction: column;
            justify-content: space-between;
            text-align: center;
            padding: 15px;
        }

        .info-card {
            background: white;
            padding: 25px;
            border-radius: 25px;
            box-shadow: 0 4px 15px rgba(0,0,0,0.05);
            border: 4px solid var(--wall-color);
        }

        h1 { font-size: 1.6rem; margin: 0 0 15px 0; color: var(--primary-color); }
        
        #instruction-text {
            font-size: 4.5rem;
            color: #D81B60;
            font-weight: bold;
            display: block;
            margin: 10px 0;
            text-shadow: 2px 2px 0px rgba(0,0,0,0.1);
        }

        .name-label { font-size: 1.4rem; color: #444; font-weight: 600; }

        #speaker-btn {
            background: var(--secondary-color);
            border: none;
            width: 100px;
            height: 100px;
            border-radius: 50%;
            font-size: 50px;
            cursor: pointer;
            box-shadow: 0 8px 0 #FBC02D;
            transition: all 0.1s;
            margin: 20px auto;
            display: flex;
            justify-content: center;
            align-items: center;
        }

        #speaker-btn:active { transform: translateY(4px); box-shadow: 0 3px 0 #FBC02D; }

        #canvas-container {
            flex-grow: 1;
            display: flex;
            justify-content: center;
            align-items: center;
            background: #fff;
            border-radius: 20px;
            border: 6px inset #f0f0f0;
            position: relative;
            overflow: hidden;
        }

        canvas {
            max-width: 100%;
            max-height: 100%;
            filter: drop-shadow(0 5px 15px rgba(0,0,0,0.1));
        }

        .controls-sidebar {
            width: 200px;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            gap: 20px;
        }

        .btn-dir {
            width: 90px;
            height: 90px;
            background: white;
            border: 5px solid var(--primary-color);
            border-radius: 25px;
            font-size: 45px;
            display: flex;
            justify-content: center;
            align-items: center;
            cursor: pointer;
            user-select: none;
            box-shadow: 0 8px 0 var(--primary-color);
            transition: all 0.1s;
        }

        .btn-dir:active { transform: translateY(4px); box-shadow: 0 3px 0 var(--primary-color); background: #F1F8E9; }

        .control-row { display: flex; gap: 20px; }

        .full-overlay {
            position: fixed;
            top: 0; left: 0; width: 100%; height: 100%;
            display: flex;
            justify-content: center;
            align-items: center;
            z-index: 1000;
        }

        #start-screen { background: linear-gradient(135deg, #2E7D32 0%, #1B5E20 100%); }

        .modal {
            background: white;
            padding: 40px;
            border-radius: 40px;
            text-align: center;
            width: 90%;
            max-width: 600px;
            border: 12px solid var(--secondary-color);
            box-shadow: 0 30px 60px rgba(0,0,0,0.4);
        }

        .guide-box {
            display: flex;
            justify-content: space-around;
            margin: 30px 0;
            text-align: left;
            background: #f9f9f9;
            padding: 20px;
            border-radius: 20px;
        }

        .guide-item { font-size: 1.1rem; color: #333; margin-bottom: 10px; display: flex; align-items: center; gap: 10px; }

        .btn-main {
            background: #FF9800;
            color: white;
            border: none;
            padding: 20px 60px;
            font-size: 2.2rem;
            border-radius: 60px;
            cursor: pointer;
            font-weight: bold;
            box-shadow: 0 10px 0 #E65100;
            transition: all 0.1s;
        }

        .btn-main:active { transform: translateY(5px); box-shadow: 0 3px 0 #E65100; }

        #win-overlay { background: rgba(0, 0, 0, 0.8); }

        @media (max-aspect-ratio: 1/1) {
            #game-wrapper { flex-direction: column; height: 95vh; width: 95vw; }
            .sidebar, .controls-sidebar { width: 100%; flex-direction: row; height: auto; }
            #instruction-text { font-size: 3rem; }
        }
    </style>
</head>
<body>

<div id="start-screen" class="full-overlay">
    <div class="modal">
        <h1 style="font-size: 3.5rem; color: #2E7D32; margin-bottom: 10px;">Assalamu'alaikum!</h1>
        <p style="font-size: 1.6rem; color: #555;">Selamat datang di <b>Labirin Hijaiyah</b></p>
        
        <div class="guide-box">
            <div>
                <div class="guide-item"><span>🎯</span> <b>Tujuan:</b> Cari huruf yang diminta!</div>
                <div class="guide-item"><span>🕹️</span> <b>Kontrol:</b> Gunakan tombol panah.</div>
                <div class="guide-item"><span>🔊</span> <b>Dengar:</b> Tekan speaker untuk suara.</div>
            </div>
        </div>

        <button id="start-btn" class="btn-main" onclick="startGame()">AYO MULAI! 🚀</button>
    </div>
</div>

<div id="game-wrapper">
    <div class="sidebar">
        <div class="info-card">
            <h1>Cari Huruf</h1>
            <span id="instruction-text">...</span>
            <div id="name-label" class="name-label">...</div>
        </div>
        <button id="speaker-btn">🔊</button>
        <div class="info-card" style="background: #E8F5E9;">
            <p style="margin:0; font-weight:bold;">Sentuh tombol panah untuk bergerak!</p>
        </div>
    </div>

    <div id="canvas-container">
        <canvas id="gameCanvas"></canvas>
    </div>

    <div class="controls-sidebar">
        <div class="btn-dir" onmousedown="handleMove('up')" ontouchstart="handleMove('up')">▲</div>
        <div class="control-row">
            <div class="btn-dir" onmousedown="handleMove('left')" ontouchstart="handleMove('left')">◀</div>
            <div class="btn-dir" onmousedown="handleMove('right')" ontouchstart="handleMove('right')">▶</div>
        </div>
        <div class="btn-dir" onmousedown="handleMove('down')" ontouchstart="handleMove('down')">▼</div>
    </div>
</div>

<div id="win-overlay" class="full-overlay" style="display: none;">
    <div class="modal" style="border-color: #4CAF50;">
        <h2 id="win-title" style="font-size: 3.5rem; color: #2E7D32;">Maa Syaa Allah! 🎉</h2>
        <p id="win-msg" style="font-size: 1.8rem; margin: 20px 0;">Jawabanmu Benar Sekali!</p>
        <button class="btn-main" style="background: #4CAF50; box-shadow: 0 10px 0 #2E7D32;" onclick="nextQuestion()">SOAL BERIKUTNYA ➔</button>
    </div>
</div>

<script>
    const canvas = document.getElementById('gameCanvas');
    const ctx = canvas.getContext('2d');
    const instructionText = document.getElementById('instruction-text');
    const nameLabel = document.getElementById('name-label');
    const winOverlay = document.getElementById('win-overlay');
    const startScreen = document.getElementById('start-screen');
    const gameWrapper = document.getElementById('game-wrapper');
    const apiKey = ""; 

    const gridSize = 7;
    let cellSize;
    let player = { x: 0, y: 0 };
    let maze = [];
    let targets = [];
    let currentQuestion = null;
    let audioEnabled = false;
    let isSpeaking = false;

    const hijaiyahData = [
        { char: 'أَ', name: 'Alif Fathah', read: 'A', desc: 'Alif harakat fathah, dibaca: A' },
        { char: 'بِ', name: 'Ba Kasrah', read: 'Bi', desc: 'Ba harakat kasrah, dibaca: Bee' },
        { char: 'تُ', name: 'Ta Dhammah', read: 'Tu', desc: 'Ta harakat dhammah, dibaca: Too' },
        { char: 'ثَ', name: 'Tsa Fathah', read: 'Tsa', desc: 'Tsa harakat fathah, dibaca: Tsa' },
        { char: 'جِ', name: 'Jim Kasrah', read: 'Ji', desc: 'Jim harakat kasrah, dibaca: Jee' },
        { char: 'حُ', name: 'Ha Dhammah', read: 'Hu', desc: 'Ha harakat dhammah, dibaca: Hoo' },
        { char: 'خَ', name: 'Kho Fathah', read: 'Kho', desc: 'Kho harakat fathah, dibaca: Khoo' },
        { char: 'دِ', name: 'Dal Kasrah', read: 'Di', desc: 'Dal harakat kasrah, dibaca: Dee' },
        { char: 'ذُ', name: 'Dzal Dhammah', read: 'Dzu', desc: 'Dzal harakat dhammah, dibaca: Dzoo' },
        { char: 'رَ', name: 'Ro Fathah', read: 'Ro', desc: 'Ro harakat fathah, dibaca: Roo' }
    ];

    async function startGame() {
        const startBtn = document.getElementById('start-btn');
        startBtn.disabled = true;
        startBtn.innerText = "MENYIAPKAN...";
        
        audioEnabled = true;
        
        // 1. Sapaan awal (tunggu sampai selesai)
        await speakText("Assalamu'alaikum warahmatullah. Selamat datang di labirin hijaiyah. Ayo bantu karakter menemukan huruf yang benar!");
        
        // Jeda singkat agar tidak kaget
        await new Promise(resolve => setTimeout(resolve, 800));
        
        startScreen.style.display = 'none';
        gameWrapper.style.visibility = 'visible';
        
        init();
    }

    function init() {
        resizeCanvas();
        generateMaze();
        nextQuestion();
    }

    function resizeCanvas() {
        const container = document.getElementById('canvas-container');
        const size = Math.min(container.clientWidth, container.clientHeight) - 40;
        canvas.width = size;
        canvas.height = size;
        cellSize = size / gridSize;
    }

    function generateMaze() {
        maze = Array(gridSize).fill().map(() => Array(gridSize).fill(1));
        function carve(x, y) {
            maze[y][x] = 0;
            const dirs = [[0,1],[0,-1],[1,0],[-1,0]].sort(() => Math.random() - 0.5);
            for(let [dx, dy] of dirs) {
                let nx = x + dx*2, ny = y + dy*2;
                if(nx >= 0 && nx < gridSize && ny >= 0 && ny < gridSize && maze[ny][nx] === 1) {
                    maze[y+dy][x+dx] = 0;
                    carve(nx, ny);
                }
            }
        }
        carve(0, 0);
        for(let i=0; i<gridSize; i++) {
            for(let j=0; j<gridSize; j++) if(Math.random() > 0.75) maze[i][j] = 0;
        }
        maze[0][0] = 0;
    }

    function nextQuestion() {
        winOverlay.style.display = 'none';
        player = { x: 0, y: 0 };
        
        const shuffled = [...hijaiyahData].sort(() => Math.random() - 0.5);
        const selected = shuffled.slice(0, 3);
        currentQuestion = selected[0];
        
        instructionText.innerText = currentQuestion.char;
        nameLabel.innerText = currentQuestion.name;
        
        speakText(`Carilah huruf: ${currentQuestion.desc}`);

        targets = [];
        let emptySpots = [];
        for(let y=0; y<gridSize; y++) {
            for(let x=0; x<gridSize; x++) {
                if(maze[y][x] === 0 && (x !== 0 || y !== 0)) emptySpots.push({x, y});
            }
        }
        
        selected.forEach((item, index) => {
            if (emptySpots.length > 0) {
                const spotIdx = Math.floor(Math.random() * emptySpots.length);
                const spot = emptySpots.splice(spotIdx, 1)[0];
                targets.push({ ...item, x: spot.x, y: spot.y, isCorrect: index === 0 });
            }
        });
        
        draw();
    }

    function draw() {
        ctx.clearRect(0, 0, canvas.width, canvas.height);
        ctx.fillStyle = '#A5D6A7';
        for(let y=0; y<gridSize; y++) {
            for(let x=0; x<gridSize; x++) {
                if(maze[y][x] === 1) {
                    ctx.beginPath();
                    ctx.roundRect(x * cellSize + 5, y * cellSize + 5, cellSize - 10, cellSize - 10, 15);
                    ctx.fill();
                }
            }
        }
        ctx.font = `bold ${cellSize * 0.6}px Arial`;
        ctx.textAlign = 'center';
        ctx.textBaseline = 'middle';
        targets.forEach(t => {
            ctx.fillStyle = '#1B5E20';
            ctx.fillText(t.char, t.x * cellSize + cellSize/2, t.y * cellSize + cellSize/2);
        });
        const px = player.x * cellSize + cellSize/2;
        const py = player.y * cellSize + cellSize/2;
        ctx.fillStyle = '#FFD54F';
        ctx.beginPath();
        ctx.arc(px, py, cellSize * 0.38, 0, Math.PI*2);
        ctx.fill();
        ctx.strokeStyle = '#F9A825';
        ctx.lineWidth = 4;
        ctx.stroke();
        ctx.fillStyle = '#333';
        ctx.beginPath(); ctx.arc(px - 10, py - 8, 5, 0, Math.PI*2); ctx.fill();
        ctx.beginPath(); ctx.arc(px + 10, py - 8, 5, 0, Math.PI*2); ctx.fill();
        ctx.strokeStyle = '#E65100';
        ctx.lineWidth = 3;
        ctx.beginPath(); ctx.arc(px, py + 5, 12, 0.1, Math.PI - 0.1); ctx.stroke();
    }

    function handleMove(dir) {
        if (!maze || maze.length === 0) return;
        let nx = player.x, ny = player.y;
        if(dir === 'up') ny--;
        if(dir === 'down') ny++;
        if(dir === 'left') nx--;
        if(dir === 'right') nx++;
        if(nx >= 0 && nx < gridSize && ny >= 0 && ny < gridSize) {
            if (maze[ny] && maze[ny][nx] === 0) {
                player.x = nx; player.y = ny;
                checkCollision();
                draw();
            }
        }
    }

    function checkCollision() {
        const target = targets.find(t => t.x === player.x && t.y === player.y);
        if(target) {
            if(target.isCorrect) {
                speakText(`Maa Syaa Allah, luar biasa! Itu adalah huruf ${currentQuestion.read}`);
                winOverlay.style.display = 'flex';
            } else {
                speakText("Ayo coba lagi, cari huruf yang tepat!");
                setTimeout(() => { player = {x:0, y:0}; draw(); }, 600);
            }
        }
    }

    async function speakText(text) {
        if(!audioEnabled || isSpeaking) return;
        isSpeaking = true;
        try {
            const response = await fetch(`https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash-preview-tts:generateContent?key=${apiKey}`, {
                method: 'POST',
                headers: { 'Content-Type': 'application/json' },
                body: JSON.stringify({
                    contents: [{ parts: [{ text: `Katakan dengan tenang, jelas, dan sangat ramah untuk anak-anak: ${text}` }] }],
                    generationConfig: {
                        responseModalities: ["AUDIO"],
                        speechConfig: { voiceConfig: { prebuiltVoiceConfig: { voiceName: "Kore" } } }
                    },
                    model: "gemini-2.5-flash-preview-tts"
                })
            });
            const result = await response.json();
            if (result.candidates?.[0]?.content?.parts?.[0]?.inlineData?.data) {
                const pcmData = result.candidates[0].content.parts[0].inlineData.data;
                await playAudio(pcmData);
            }
        } catch (e) { console.error("TTS Error:", e); }
        finally { isSpeaking = false; }
    }

    function playAudio(base64) {
        return new Promise((resolve) => {
            const bin = atob(base64);
            const bytes = new Uint8Array(bin.length);
            for (let i = 0; i < bin.length; i++) bytes[i] = bin.charCodeAt(i);
            const wav = createWavHeader(bin.length, 24000);
            const blob = new Blob([wav, bytes], { type: 'audio/wav' });
            const audio = new Audio(URL.createObjectURL(blob));
            audio.onended = () => resolve();
            audio.onerror = () => resolve();
            audio.play();
        });
    }

    function createWavHeader(len, rate) {
        const b = new ArrayBuffer(44);
        const v = new DataView(b);
        v.setUint32(0, 0x52494646, false); v.setUint32(4, 36+len, true);
        v.setUint32(8, 0x57415645, false); v.setUint32(12, 0x666d7420, false);
        v.setUint32(16, 16, true); v.setUint16(20, 1, true); v.setUint16(22, 1, true);
        v.setUint32(24, rate, true); v.setUint32(28, rate*2, true);
        v.setUint16(32, 2, true); v.setUint16(34, 16, true);
        v.setUint32(36, 0x64617461, false); v.setUint32(40, len, true);
        return b;
    }

    document.getElementById('speaker-btn').onclick = () => speakText(`Sekali lagi, cari huruf ${currentQuestion.desc}`);
    window.onresize = resizeCanvas;
    window.addEventListener('keydown', (e) => {
        if(e.key.includes('Arrow')) {
            e.preventDefault();
            handleMove(e.key.replace('Arrow','').toLowerCase());
        }
    });
</script>
</body>
</html>
