<!DOCTYPE html>
<html lang="zh-TW">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>粉嫩糖果版 - 認字找找看</title>
    <script src="https://cdn.jsdelivr.net/npm/hanzi-writer@3.5/dist/hanzi-writer.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/canvas-confetti@1.6.0/dist/confetti.browser.min.js"></script>
    <style>
        :root { 
            --p-blue: #d1e9ff;   /* 粉藍 */
            --p-yellow: #fff9db; /* 粉黃 */
            --p-orange: #ffe8cc; /* 粉橘 */
            --p-green: #d3f9d8;  /* 粉綠 */
            --text-dark: #495057;  /* 深灰字 */
            --text-light: #868e96; /* 淺灰字 */
            --accent-blue: #74c0fc;
            --accent-green: #69db7c;
            --accent-orange: #ffa94d;
        }

        body { 
            font-family: 'PingFang TC', 'Microsoft JhengHei', sans-serif; 
            background: linear-gradient(135deg, #fffcf0 0%, #f0f7ff 100%); 
            color: var(--text-dark);
            display: flex; 
            flex-direction: column; 
            align-items: center; 
            padding: 15px; 
            margin: 0; 
            min-height: 100vh;
        }
        
        .main-container { 
            background: rgba(255, 255, 255, 0.9); 
            padding: 25px; 
            border-radius: 30px; 
            box-shadow: 0 10px 25px rgba(0,0,0,0.05); 
            width: 100%; 
            max-width: 500px; 
            box-sizing: border-box; 
            margin-bottom: 20px; 
            text-align: center;
            border: 2px solid #fff;
        }
        .hidden { display: none !important; }

        /* 標籤頁 */
        .nav-tabs { display: flex; gap: 10px; margin-bottom: 25px; }
        .tab { 
            flex: 1; 
            text-align: center; 
            padding: 12px; 
            cursor: pointer; 
            font-weight: bold; 
            color: var(--text-light); 
            background: #f8f9fa;
            border-radius: 15px;
            transition: 0.3s;
        }
        .tab.active { 
            color: var(--text-dark); 
            background: var(--p-blue);
            box-shadow: inset 0 -3px 0 rgba(0,0,0,0.05);
        }
        
        /* 表單元素 */
        label { display: block; text-align: left; margin-left: 5px; font-weight: bold; color: var(--text-light); font-size: 14px; }
        input, textarea, select { 
            width: 100%; padding: 12px; margin: 8px 0; 
            border: 2px solid #f1f3f5; border-radius: 15px; 
            box-sizing: border-box; font-size: 16px; color: var(--text-dark);
            background: #fff;
        }
        input:focus { outline: none; border-color: var(--p-blue); }
        
        /* 按鈕樣式 */
        button { 
            cursor: pointer; border: none; border-radius: 15px; 
            font-weight: bold; padding: 12px; transition: 0.2s; 
            color: var(--text-dark);
        }
        button:active { transform: scale(0.96); }
        .btn-action { background: var(--p-green); flex: 1; }
        .btn-start { background: var(--p-blue); width: 100%; margin-top: 15px; font-size: 18px; padding: 15px; }

        /* 字卡模式 */
        #writer-target { 
            background: #fff; border: 4px solid var(--p-yellow); 
            border-radius: 25px; margin: 15px 0; 
            box-shadow: 0 5px 15px rgba(0,0,0,0.03);
        }
        .speed-ctrl { accent-color: var(--accent-blue); width: 80%; }
        .word-picker { display: flex; flex-wrap: wrap; gap: 8px; justify-content: center; margin-top: 20px; }
        .pick-item { 
            padding: 10px 15px; background: white; border: 2px solid var(--p-orange); 
            border-radius: 12px; cursor: pointer; font-weight: bold; color: var(--text-dark);
        }
        .pick-item.active { background: var(--p-orange); }

        /* 遊戲方格 */
        .grid-container { 
            display: grid; gap: 10px; background: var(--p-yellow); 
            padding: 15px; border-radius: 25px; margin: 15px auto; 
            width: 100%; aspect-ratio: 1 / 1; box-sizing: border-box;
        }
        .cell { 
            background: white; border-radius: 15px; 
            display: flex; align-items: center; justify-content: center; 
            font-weight: bold; cursor: pointer; border: 2px solid #fff9db;
            color: var(--text-dark); transition: 0.1s;
        }
        .size-3 { font-size: 65px; }
        .size-4 { font-size: 48px; }
        .size-5 { font-size: 38px; }
        .size-6 { font-size: 30px; }
        .size-7 { font-size: 24px; }

        .cell.correct { background: var(--accent-green) !important; color: white; border-color: var(--accent-green); }
        .cell.wrong { background: var(--accent-orange) !important; color: white; border-color: var(--accent-orange); animation: shake 0.3s; }

        @keyframes shake { 0%, 100% {transform: translateX(0);} 25% {transform: translateX(-5px);} 75% {transform: translateX(5px);} }

        /* 過關彈窗 */
        #winOverlay { 
            position: fixed; top: 0; left: 0; width: 100%; height: 100%; 
            background: rgba(255,255,255,0.8); backdrop-filter: blur(5px);
            display: flex; align-items: center; justify-content: center; z-index: 100; visibility: hidden; opacity: 0; transition: 0.5s; 
        }
        #winOverlay.show { visibility: visible; opacity: 1; }
        .win-card { 
            background: white; padding: 40px; border-radius: 40px; text-align: center; 
            box-shadow: 0 20px 50px rgba(0,0,0,0.1); border: 5px solid var(--p-green);
        }
    </style>
</head>
<body>

    <div class="main-container" id="editorPanel">
        <div class="nav-tabs">
            <div class="tab active" id="tab-study" onclick="switchTab('study')">📖 學習模式</div>
            <div class="tab" id="tab-game" onclick="switchTab('game')">🎮 找字遊戲</div>
        </div>

        <label>選擇已有課程</label>
        <select id="lessonSelect" onchange="loadLesson()"></select>
        
        <label>課程名稱</label>
        <input type="text" id="lessonTitle" placeholder="例如：水果名稱">
        
        <label>輸入生字 (請用逗號隔開)</label>
        <textarea id="wordsInput" rows="3" placeholder="例如：蘋果,香蕉,西瓜"></textarea>
        
        <div class="btn-group">
            <button class="btn-action" onclick="saveData()">💾 儲存課程</button>
            <button style="background:var(--p-orange); margin-left:8px;" onclick="exportData()">📤 匯出</button>
            <button style="background:var(--p-orange); margin-left:8px;" onclick="document.getElementById('importFile').click()">📥 匯入</button>
            <input type="file" id="importFile" class="hidden" onchange="importData(event)">
        </div>

        <div id="studyConfig">
            <button class="btn-start" onclick="startStudy()">進入筆順學習 ✨</button>
        </div>

        <div id="gameConfig" class="hidden">
            <label>挑戰難度</label>
            <select id="gridSize">
                <option value="3">3x3 (入門)</option>
                <option value="4" selected>4x4 (標準)</option>
                <option value="5">5x5 (進階)</option>
                <option value="6">6x6 (專家)</option>
                <option value="7">7x7 (挑戰)</option>
            </select>
            <button class="btn-start" onclick="startGame()">開始挑戰 🚀</button>
        </div>
    </div>

    <div class="main-container hidden" id="flashcardView">
        <div id="writer-target" style="margin: 0 auto;" onclick="speak('study')"></div>
        <h1 id="currentWord" style="margin:10px 0; font-size:48px; color:var(--text-dark); cursor:pointer;" onclick="speak('study')">-</h1>
        
        <label>🐢 慢 &nbsp; 筆順速度 &nbsp; 快 🐇</label>
        <input type="range" id="speedRange" class="speed-ctrl" min="0.5" max="3" step="0.5" value="1" onchange="updateFlashcard()">

        <div class="word-picker" id="wordPicker"></div>
        <button onclick="exitToMenu()" style="width:100%; margin-top:25px; background:#f1f3f5;">◀ 返回主選單</button>
    </div>

    <div class="main-container hidden" id="gameView">
        <div style="font-size: 24px; color: var(--accent-orange); font-weight: bold;">⏱ <span id="timeVal">0.0</span>s</div>
        <div style="margin: 15px 0;">
            <div style="color:var(--text-light); font-size:16px; margin-bottom:5px;">請找出：</div>
            <span id="gameTarget" onclick="speak('game')" style="font-size:64px; color:var(--accent-blue); font-weight:bold; cursor:pointer; text-shadow: 2px 2px 0px #fff;">?</span>
        </div>
        <div id="grid" class="grid-container"></div>
        <div id="status" style="margin-top:15px; font-weight:bold; font-size:18px; color:var(--text-light);">剩餘目標：-</div>
        <button onclick="exitToMenu()" style="margin-top:25px; width:100%; background:#f1f3f5;">放棄挑戰</button>
    </div>

    <div id="winOverlay">
        <div class="win-card">
            <h1 style="color:var(--accent-green); font-size: 48px; margin: 0;">🌟 太棒了</h1>
            <p style="color:var(--text-light); font-size: 20px;">你已經學會這些字了！</p>
            <div style="font-size: 28px; margin: 25px 0; font-weight: bold; color:var(--text-dark);">時間：<span id="finalTime">0.0</span> 秒</div>
            <button onclick="closeWinOverlay()" style="background:var(--p-blue); padding:15px 50px; font-size:20px;">繼續練習</button>
        </div>
    </div>

    <script>
        let db = JSON.parse(localStorage.getItem('study_candy_db') || '{}');
        let studyList = [], studyIdx = 0, writerInstance = null;
        let gameTimer, gameStart, targetChar, foundNum, targetTotal;
        let audioCtx = null;

        function refreshDropdown() {
            const select = document.getElementById('lessonSelect');
            select.innerHTML = '<option value="">-- 點此新增課程 --</option>';
            Object.keys(db).forEach(name => {
                const wordCount = db[name].split(/[,，\s\n]+/).length;
                let level = wordCount > 10 ? "🔥" : (wordCount > 5 ? "⭐" : "🌱");
                select.innerHTML += `<option value="${name}">${level} ${name}</option>`;
            });
        }

        function loadLesson() {
            const name = document.getElementById('lessonSelect').value;
            if (name) {
                document.getElementById('lessonTitle').value = name;
                document.getElementById('wordsInput').value = db[name];
            }
        }

        function switchTab(mode) {
            document.getElementById('tab-study').classList.toggle('active', mode === 'study');
            document.getElementById('tab-game').classList.toggle('active', mode === 'game');
            document.getElementById('studyConfig').classList.toggle('hidden', mode !== 'study');
            document.getElementById('gameConfig').classList.toggle('hidden', mode !== 'game');
        }

        function saveData() {
            const title = document.getElementById('lessonTitle').value.trim();
            const words = document.getElementById('wordsInput').value.trim();
            if(!title || !words) return alert("請填寫課程名稱與內容");
            db[title] = words;
            localStorage.setItem('study_candy_db', JSON.stringify(db));
            refreshDropdown();
            alert("課程已成功儲存！");
        }

        function playTone(freq, duration) {
            if (!audioCtx) audioCtx = new (window.AudioContext || window.webkitAudioContext)();
            const osc = audioCtx.createOscillator();
            const gain = audioCtx.createGain();
            osc.frequency.value = freq;
            gain.gain.setValueAtTime(0.1, audioCtx.currentTime);
            gain.gain.exponentialRampToValueAtTime(0.01, audioCtx.currentTime + duration);
            osc.connect(gain);
            gain.connect(audioCtx.destination);
            osc.start();
            osc.stop(audioCtx.currentTime + duration);
        }

        function startStudy() {
            const input = document.getElementById('wordsInput').value.trim();
            studyList = input.split(/[,，\s\n]+/).filter(w => w !== "");
            if(studyList.length === 0) return alert("請先輸入一些生字");
            document.getElementById('editorPanel').classList.add('hidden');
            document.getElementById('flashcardView').classList.remove('hidden');
            const picker = document.getElementById('wordPicker');
            picker.innerHTML = "";
            studyList.forEach((w, idx) => {
                const span = document.createElement('div');
                span.className = 'pick-item';
                span.innerText = w;
                span.onclick = () => { studyIdx = idx; updateFlashcard(); };
                picker.appendChild(span);
            });
            studyIdx = 0;
            updateFlashcard();
        }

        function updateFlashcard() {
            const char = studyList[studyIdx][0];
            const speed = document.getElementById('speedRange').value;
            document.getElementById('currentWord').innerText = studyList[studyIdx];
            document.getElementById('writer-target').innerHTML = "";
            const items = document.querySelectorAll('.pick-item');
            items.forEach((item, i) => item.classList.toggle('active', i === studyIdx));

            if (window.HanziWriter) {
                writerInstance = HanziWriter.create('writer-target', char, {
                    width: 220, height: 220, padding: 10,
                    strokeAnimationSpeed: speed,
                    delayBetweenStrokes: 150 / speed,
                    strokeColor: '#495057'
                });
                writerInstance.animateCharacter();
            }
            speak('study');
        }

        function startGame() {
            const input = document.getElementById('wordsInput').value.trim();
            const wordPool = input.split(/[,，\s\n]+/).filter(w => w !== "");
            if(wordPool.length === 0) return alert("請先輸入生字");
            
            targetChar = wordPool[Math.floor(Math.random() * wordPool.length)][0];
            const size = parseInt(document.getElementById('gridSize').value);
            targetTotal = size; 
            foundNum = 0;

            let gridPool = [];
            for(let i=0; i<targetTotal; i++) gridPool.push(targetChar);
            while(gridPool.length < size * size) {
                let rChar = wordPool[Math.floor(Math.random() * wordPool.length)][0];
                if(rChar !== targetChar) gridPool.push(rChar);
                else gridPool.push("★");
            }
            gridPool.sort(() => Math.random() - 0.5);

            document.getElementById('editorPanel').classList.add('hidden');
            document.getElementById('gameView').classList.remove('hidden');
            document.getElementById('gameTarget').innerText = targetChar;
            
            const container = document.getElementById('grid');
            container.innerHTML = "";
            container.style.gridTemplateColumns = `repeat(${size}, 1fr)`;
            document.getElementById('status').innerText = `剩餘目標：${targetTotal}`;

            gridPool.forEach(char => {
                const div = document.createElement('div');
                div.className = `cell size-${size}`;
                div.innerText = char;
                div.onclick = () => {
                    if(char === targetChar && !div.classList.contains('correct')) {
                        div.classList.add('correct');
                        playTone(900, 0.1);
                        foundNum++;
                        document.getElementById('status').innerText = `剩餘目標：${targetTotal - foundNum}`;
                        if(foundNum === targetTotal) handleWin();
                    } else if(char !== targetChar) {
                        playTone(300, 0.2);
                        div.classList.add('wrong');
                        setTimeout(() => div.classList.remove('wrong'), 300);
                    }
                };
                container.appendChild(div);
            });
            startTimer();
            speak('game');
        }

        function handleWin() {
            clearInterval(gameTimer);
            document.getElementById('finalTime').innerText = document.getElementById('timeVal').innerText;
            playTone(523, 0.1); setTimeout(()=>playTone(659,0.1), 100); setTimeout(()=>playTone(783,0.3), 200);
            confetti({ particleCount: 200, spread: 80, origin: { y: 0.6 } });
            setTimeout(() => { document.getElementById('winOverlay').classList.add('show'); }, 500);
        }

        function closeWinOverlay() {
            document.getElementById('winOverlay').classList.remove('show');
            exitToMenu();
        }

        function startTimer() {
            gameStart = Date.now();
            gameTimer = setInterval(() => {
                document.getElementById('timeVal').innerText = ((Date.now() - gameStart) / 1000).toFixed(1);
            }, 100);
        }

        function speak(mode) {
            const text = (mode === 'study') ? studyList[studyIdx] : targetChar;
            const msg = new SpeechSynthesisUtterance(text);
            msg.lang = 'zh-TW';
            window.speechSynthesis.speak(msg);
        }

        function exitToMenu() {
            clearInterval(gameTimer);
            document.getElementById('editorPanel').classList.remove('hidden');
            document.getElementById('flashcardView').classList.add('hidden');
            document.getElementById('gameView').classList.add('hidden');
        }

        function exportData() {
            const blob = new Blob([JSON.stringify(db)], {type: "application/json"});
            const a = document.createElement('a');
            a.href = URL.createObjectURL(blob);
            a.download = "我的課程備份.json";
            a.click();
        }

        function importData(e) {
            const reader = new FileReader();
            reader.onload = (ev) => {
                db = {...db, ...JSON.parse(ev.target.result)};
                localStorage.setItem('study_candy_db', JSON.stringify(db));
                refreshDropdown();
                alert("課程匯入成功！");
            };
            reader.readAsText(e.target.files[0]);
        }

        refreshDropdown();
    </script>
</body>
</html>
