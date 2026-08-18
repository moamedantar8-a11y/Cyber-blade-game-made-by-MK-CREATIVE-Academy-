<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
    <title>MK CREATIVE Agency - Pro Arcade Hub</title>
    <style>
        :root {
            --bg-color: #030712;
            --container-width: 420px;
            --game-height: 220px;
            --accent: #00ffcc;
        }

        body {
            background-color: var(--bg-color);
            color: #fff;
            text-align: center;
            font-family: 'Segoe UI', Tahoma, sans-serif;
            margin: 0;
            display: flex;
            flex-direction: column;
            justify-content: flex-start;
            align-items: center;
            min-height: 100vh;
            overflow-x: hidden;
            padding: 15px 0;
            touch-action: manipulation;
            transition: background 0.3s;
        }

        /* شاشة التحميل الخاصة بالشركة (Loading Screen) */
        #loader-screen {
            position: fixed;
            top: 0; left: 0;
            width: 100vw; height: 100vh;
            background: #030712;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            z-index: 9999;
            transition: opacity 0.5s ease;
        }
        .loader-logo {
            font-size: 2.2rem;
            font-weight: bold;
            color: var(--accent);
            text-shadow: 0 0 20px rgba(0,255,204,0.6);
            margin-bottom: 15px;
            letter-spacing: 2px;
        }
        .loader-spinner {
            width: 45px; height: 45px;
            border: 4px solid #1e293b;
            border-top: 4px solid var(--accent);
            border-radius: 50%;
            animation: spin 0.8s linear infinite;
            margin-bottom: 15px;
        }
        .loader-text {
            color: #94a3b8;
            font-size: 0.9rem;
            letter-spacing: 1px;
        }
        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }

        .top-bar {
            display: flex;
            justify-content: space-between;
            align-items: center;
            width: 90vw;
            max-width: 700px;
            margin-bottom: 20px;
        }

        h1.main-title {
            color: var(--accent);
            font-size: 1.5rem;
            margin: 0;
            text-shadow: 0 0 15px rgba(0,255,204,0.5);
        }

        .top-controls {
            display: flex;
            gap: 8px;
        }

        .control-btn, .mode-toggle-btn {
            background: #1e293b;
            color: var(--accent);
            border: 1px solid var(--accent);
            padding: 6px 12px;
            border-radius: 6px;
            font-size: 0.8rem;
            font-weight: bold;
            cursor: pointer;
            box-shadow: 0 0 8px rgba(0,255,204,0.2);
        }

        /* لوحة التحكم الرئيسية (Dashboard Menu) */
        #main-menu {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
            gap: 15px;
            width: 90vw;
            max-width: 700px;
            margin-bottom: 20px;
        }

        .game-card {
            background: #1e293b;
            border: 2px solid #334155;
            border-radius: 12px;
            padding: 18px 12px;
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 8px;
            cursor: pointer;
            transition: all 0.2s ease;
            box-shadow: 0 4px 12px rgba(0,0,0,0.3);
        }

        .game-card:hover {
            border-color: var(--accent);
            transform: translateY(-4px);
            box-shadow: 0 0 15px rgba(0,255,204,0.3);
        }

        .game-card-icon {
            font-size: 2.2rem;
        }

        .game-card-title {
            font-size: 0.95rem;
            font-weight: bold;
            color: #fff;
            margin: 0;
        }

        .game-card-desc {
            font-size: 0.75rem;
            color: #94a3b8;
            margin: 0;
        }

        /* حاوية الألعاب (تظهر عند اختيار لعبة) */
        .game-section {
            display: none;
            width: var(--container-width);
            flex-direction: column;
            align-items: center;
            transition: width 0.3s ease;
        }
        .game-section.active {
            display: flex;
        }

        .score-board {
            width: 100%;
            display: flex;
            justify-content: space-between;
            margin-bottom: 10px;
            font-size: 0.95rem;
            font-weight: bold;
            color: #cbd5e1;
        }
        .score-board span { color: var(--accent); }

        /* لعبة 1: Cyber Jump */
        #game-container {
            width: 100%;
            height: var(--game-height);
            border: 2px solid var(--accent);
            position: relative;
            background: linear-gradient(to bottom, #0f172a, #1e293b);
            overflow: hidden;
            border-radius: 10px;
            box-shadow: 0 0 20px rgba(0,255,204,0.25);
        }
        #hero {
            width: 28px; height: 28px;
            background: var(--accent);
            position: absolute;
            bottom: 0; left: 40px;
            border-radius: 4px;
            box-shadow: 0 0 10px var(--accent);
        }
        #block {
            width: 20px; height: 38px;
            background: #ff3366;
            position: absolute;
            bottom: 0; left: 100%;
            border-radius: 4px;
            box-shadow: 0 0 10px #ff3366;
        }
        .jump {
            animation: heroJump 0.4s cubic-bezier(0,0,0.2,1);
        }
        @keyframes heroJump {
            0% { bottom: 0; }
            50% { bottom: 110px; }
            100% { bottom: 0; }
        }
        
        #game-over {
            position: absolute;
            top: 0; left: 0; width: 100%; height: 100%;
            background: rgba(3, 7, 18, 0.92);
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            display: none;
            z-index: 20;
        }
        #game-over h2 { color: #ff3366; font-size: 1.5rem; margin-bottom: 10px; }
        
        .action-btn {
            padding: 10px 24px;
            background: var(--accent);
            color: #030712;
            border: none;
            font-weight: bold;
            border-radius: 6px;
            font-size: 0.95rem;
            cursor: pointer;
            box-shadow: 0 0 15px rgba(0,255,204,0.5);
            z-index: 30;
            -webkit-tap-highlight-color: transparent;
        }

        /* لعبة 2: Memory Match */
        .memory-grid {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 8px;
            width: 100%;
            margin-bottom: 12px;
        }
        .memory-card {
            aspect-ratio: 1;
            background: #1e293b;
            border: 2px solid #334155;
            border-radius: 8px;
            font-size: 1.4rem;
            display: flex;
            justify-content: center;
            align-items: center;
            cursor: pointer;
            user-select: none;
        }
        .memory-card.flipped {
            background: #0f172a;
            border-color: var(--accent);
        }

        /* تنسيقات الألعاب الفرعية */
        .mini-game-card {
            width: 100%;
            background: #1e293b;
            border: 2px solid #334155;
            border-radius: 10px;
            padding: 15px;
            box-sizing: border-box;
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 12px;
            box-shadow: 0 0 15px rgba(0,0,0,0.4);
        }
        .mini-game-card input {
            width: 80%;
            padding: 10px;
            background: #0f172a;
            border: 1px solid var(--accent);
            color: #fff;
            border-radius: 6px;
            text-align: center;
            font-size: 1.1rem;
            outline: none;
        }
        .rps-choices {
            display: flex;
            gap: 12px;
            justify-content: center;
        }
        .rps-btn {
            font-size: 1.8rem;
            padding: 10px 18px;
            background: #0f172a;
            border: 2px solid #334155;
            border-radius: 8px;
            cursor: pointer;
            transition: 0.2s;
            -webkit-tap-highlight-color: transparent;
        }
        .rps-btn:hover {
            border-color: var(--accent);
            transform: scale(1.05);
        }

        /* شبكة إكس أوت */
        .ttt-grid {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 6px;
            width: 100%;
            max-width: 220px;
            margin: 0 auto;
        }
        .ttt-cell {
            aspect-ratio: 1;
            background: #0f172a;
            border: 2px solid #334155;
            border-radius: 8px;
            font-size: 1.8rem;
            font-weight: bold;
            display: flex;
            justify-content: center;
            align-items: center;
            cursor: pointer;
            color: var(--accent);
        }

        /* لعبة 7: Space Dodge */
        #space-container {
            width: 100%;
            height: var(--game-height);
            border: 2px solid var(--accent);
            position: relative;
            background: #050b14;
            overflow: hidden;
            border-radius: 10px;
        }
        #spaceship {
            width: 30px; height: 20px;
            background: var(--accent);
            position: absolute;
            bottom: 10px; left: 50%;
            transform: translateX(-50%);
            border-radius: 4px;
        }

        /* لعبة 8: Fast Clicker */
        .clicker-box {
            font-size: 3rem;
            background: #0f172a;
            border: 2px dashed var(--accent);
            border-radius: 50%;
            width: 110px; height: 110px;
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            user-select: none;
            transition: 0.1s;
        }
        .clicker-box:active {
            transform: scale(0.92);
            background: var(--accent);
            color: #030712;
        }

        .hint {
            margin-top: 12px;
            color: #94a3b8;
            font-size: 0.8rem;
        }
        .copyrights {
            margin-top: 25px;
            color: #64748b;
            font-size: 0.75rem;
        }
        .copyrights span { color: var(--accent); font-weight: bold; }
    </style>
</head>
<body>

    <div id="loader-screen">
        <div class="loader-logo">MK CREATIVE AGENCY</div>
        <div class="loader-spinner"></div>
        <div class="loader-text">Loading Arcade Hub...</div>
    </div>

    <div class="top-bar">
        <h1 class="main-title">MK Arcade Hub</h1>
        <div class="top-controls">
            <button class="control-btn" id="back-home-btn" style="display:none;" onclick="goHome()">🏠 القائمة</button>
            <button class="mode-toggle-btn" onclick="toggleDesktopMode()" id="mode-btn">💻 الكمبيوتر: OFF</button>
        </div>
    </div>

    <div id="main-menu">
        <div class="game-card" onclick="openGame(1)">
            <div class="game-card-icon">🎮</div>
            <h3 class="game-card-title">Cyber Jump</h3>
            <p class="game-card-desc">تحدي القفز وتفادي العوائق</p>
        </div>
        <div class="game-card" onclick="openGame(2)">
            <div class="game-card-icon">🧠</div>
            <h3 class="game-card-title">Memory Match</h3>
            <p class="game-card-desc">اختبر قوة ذاكرتك بالرموز</p>
        </div>
        <div class="game-card" onclick="openGame(3)">
            <div class="game-card-icon">🔢</div>
            <h3 class="game-card-title">Guess Number</h3>
            <p class="game-card-desc">اعثر على الرقم السحري</p>
        </div>
        <div class="game-card" onclick="openGame(4)">
            <div class="game-card-icon">✂️</div>
            <h3 class="game-card-title">R P S</h3>
            <p class="game-card-desc">حجر، ورقة، ومقص ذكي</p>
        </div>
        <div class="game-card" onclick="openGame(5)">
            <div class="game-card-icon">❌</div>
            <h3 class="game-card-title">Tic Tac Toe</h3>
            <p class="game-card-desc">إكس أوت ضد الكمبيوتر</p>
        </div>
        <div class="game-card" onclick="openGame(6)">
            <div class="game-card-icon">🎲</div>
            <h3 class="game-card-title">Dice Roll</h3>
            <p class="game-card-desc">تحدي رمي النرد والحظ</p>
        </div>
        <div class="game-card" onclick="openGame(7)">
            <div class="game-card-icon">🚀</div>
            <h3 class="game-card-title">Space Dodge</h3>
            <p class="game-card-desc">تفادي نيازك الفضاء</p>
        </div>
        <div class="game-card" onclick="openGame(8)">
            <div class="game-card-icon">⚡</div>
            <h3 class="game-card-title">Fast Clicker</h3>
            <p class="game-card-desc">اختبر سرعة أصابعك</p>
        </div>
    </div>

    <div id="section-1" class="game-section">
        <div class="score-board">
            <div>النقاط: <span id="score">0</span></div>
            <div>الأعلى: <span id="high-score">0</span></div>
        </div>
        <div id="game-container">
            <div id="hero"></div>
            <div id="block"></div>
            <div id="game-over">
                <h2>انتهت اللعبة</h2>
                <button class="action-btn" id="restart-btn">إعادة المحاولة</button>
            </div>
        </div>
        <div class="hint">اضغط مسافة أو انقر بالشاشة للقفز! 👆</div>
    </div>

    <div id="section-2" class="game-section">
        <div class="score-board">
            <div>الحالة: <span id="mem-status">طابق الرموز المتشابهة</span></div>
        </div>
        <div class="memory-grid" id="memory-grid"></div>
        <button class="action-btn" onclick="initMemoryGame()">لعبة جديدة</button>
        <div class="hint">اكتشف كروت الرموز المتطابقة! 🧠</div>
    </div>

    <div id="section-3" class="game-section">
        <div class="score-board">
            <div>التحدي: <span id="guess-status">اختر رقماً من 1 إلى 50</span></div>
        </div>
        <div class="mini-game-card">
            <p style="margin: 0; color: #cbd5e1; font-size: 0.9rem;">حاول تخمين الرقم السحري:</p>
            <input type="number" id="guess-input" placeholder="اكتب رقمك هنا..." min="1" max="50">
            <button class="action-btn" onclick="checkGuess()">تحقق من الرقم</button>
            <p id="guess-feedback" style="margin: 0; color: var(--accent); font-weight: bold;"></p>
        </div>
        <button class="action-btn" style="margin-top: 10px; background: #334155; color: #fff;" onclick="initGuessGame()">لعبة جديدة</button>
        <div class="hint">استخدم التلميحات لتصل للرقم الصحيح! 🎯</div>
    </div>

    <div id="section-4" class="game-section">
        <div class="score-board">
            <div>أنت: <span id="player-score">0</span> | الكمبيوتر: <span id="ai-score">0</span></div>
        </div>
        <div class="mini-game-card">
            <p id="rps-result-text" style="margin: 0; color: #cbd5e1; font-size: 0.95rem;">اختر سلاحك واهزم الذكاء الاصطناعي!</p>
            <div class="rps-choices">
                <button class="rps-btn" onclick="playRPS('rock')">🪨</button>
                <button class="rps-btn" onclick="playRPS('paper')">📄</button>
                <button class="rps-btn" onclick="playRPS('scissors')">✂️</button>
            </div>
            <div id="rps-details" style="font-size: 0.85rem; color: #94a3b8;"></div>
        </div>
        <div class="hint">اختر حجر، ورقة، أو مقص للمواجهة! ⚡</div>
    </div>

    <div id="section-5" class="game-section">
        <div class="score-board">
            <div>الحالة: <span id="ttt-status">دورك (X)</span></div>
        </div>
        <div class="mini-game-card">
            <div class="ttt-grid" id="ttt-board">
                <div class="ttt-cell" onclick="makeMove(0)"></div>
                <div class="ttt-cell" onclick="makeMove(1)"></div>
                <div class="ttt-cell" onclick="makeMove(2)"></div>
                <div class="ttt-cell" onclick="makeMove(3)"></div>
                <div class="ttt-cell" onclick="makeMove(4)"></div>
                <div class="ttt-cell" onclick="makeMove(5)"></div>
                <div class="ttt-cell" onclick="makeMove(6)"></div>
                <div class="ttt-cell" onclick="makeMove(7)"></div>
                <div class="ttt-cell" onclick="makeMove(8)"></div>
            </div>
        </div>
        <button class="action-btn" style="margin-top: 10px;" onclick="resetTTT()">إعادة اللعبة</button>
        <div class="hint">حقق 3 في خط متصل لفوز سريع! ❌⭕</div>
    </div>

    <div id="section-6" class="game-section">
        <div class="score-board">
            <div>انت: <span id="dice-p">0</span> | الكمبيوتر: <span id="dice-c">0</span></div>
        </div>
        <div class="mini-game-card">
            <h2 id="dice-display" style="font-size: 3.5rem; margin: 0; color: var(--accent);">🎲</h2>
            <p id="dice-msg" style="margin: 0; color: #cbd5e1; font-size: 0.9rem;">ارْمِ النرد واكسب النقاط!</p>
            <button class="action-btn" onclick="rollDice()">ارْمِ النرد الآن</button>
        </div>
        <div class="hint">الأعلى رمية يفوز بالجولة! 🎲</div>
    </div>

    <div id="section-7" class="game-section">
        <div class="score-board">
            <div>النقاط: <span id="dodge-score">0</span></div>
        </div>
        <div id="space-container">
            <div id="spaceship"></div>
        </div>
        <div class="hint" style="margin-top: 8px;">حرك الماوس أو اسحب لتفادي النيازك! 🚀</div>
    </div>

    <div id="section-8" class="game-section">
        <div class="score-board">
            <div>الوقت: <span id="clicker-time">10</span>ث | النقرات: <span id="clicker-score">0</span></div>
        </div>
        <div class="mini-game-card">
            <div class="clicker-box" onclick="hitClicker()">🔥</div>
            <p id="clicker-msg" style="margin: 0; color: #cbd5e1; font-size: 0.9rem;">انقر بأسرع ما يمكنك قبل انتهاء الوقت!</p>
        </div>
        <button class="action-btn" style="margin-top: 10px;" onclick="startClickerGame()">ابدأ التحدي</button>
        <div class="hint">اختبر سرعة أصابعك! ⚡</div>
    </div>

    <div class="copyrights">
        © 2026 <span>MK CREATIVE Agency</span>. All Rights Reserved.
    </div>

    <script>
        /* إخفاء شاشة التحميل */
        window.addEventListener("load", () => {
            setTimeout(() => {
                const loader = document.getElementById("loader-screen");
                loader.style.opacity = "0";
                setTimeout(() => loader.style.display = "none", 500);
            }, 1200);
        });

        /* التنقل بين القائمة الرئيسية والألعاب */
        function openGame(gameNum) {
            document.getElementById("main-menu").style.display = "none";
            document.getElementById("back-home-btn").style.display = "inline-block";
            document.querySelectorAll('.game-section').forEach(sec => sec.classList.remove('active'));
            document.getElementById(`section-${gameNum}`).classList.add('active');
        }

        function goHome() {
            document.querySelectorAll('.game-section').forEach(sec => sec.classList.remove('active'));
            document.getElementById("main-menu").style.display = "grid";
            document.getElementById("back-home-btn").style.display = "none";
        }

        /* نظام وضع الكمبيوتر */
        let isDesktop = false;
        function toggleDesktopMode() {
            isDesktop = !isDesktop;
            const root = document.documentElement;
            const modeBtn = document.getElementById("mode-btn");

            if (isDesktop) {
                root.style.setProperty('--container-width', '680px');
                root.style.setProperty('--game-height', '260px');
                modeBtn.innerText = "💻 الكمبيوتر: ON";
                modeBtn.style.background = "var(--accent)";
                modeBtn.style.color = "#030712";
            } else {
                root.style.setProperty('--container-width', '420px');
                root.style.setProperty('--game-height', '220px');
                modeBtn.innerText = "💻 الكمبيوتر: OFF";
                modeBtn.style.background = "#1e293b";
                modeBtn.style.color = "var(--accent)";
            }
        }

        /* 1: Cyber Jump */
        const hero = document.getElementById("hero");
        const block = document.getElementById("block");
        const scoreElem = document.getElementById("score");
        const highScoreElem = document.getElementById("high-score");
        const gameOverScreen = document.getElementById("game-over");
        const gameContainer = document.getElementById("game-container");
        const restartBtn = document.getElementById("restart-btn");

        let score = 0;
        let highScore = localStorage.getItem("mk_desktop_high") || 0;
        highScoreElem.innerText = highScore;
        let isPlaying = true;
        let blockLeft = 400;
        let gameSpeed = 6;
        let gameInterval;

        function jump() {
            if (!hero.classList.contains("jump") && isPlaying) {
                hero.classList.add("jump");
                setTimeout(() => hero.classList.remove("jump"), 400);
            }
        }
        gameContainer.addEventListener("touchstart", (e) => { e.preventDefault(); jump(); }, {passive: false});
        gameContainer.addEventListener("click", () => jump());
        document.addEventListener("keydown", (e) => { if (e.code === "Space") { jump(); e.preventDefault(); } });

        function startGameLoop() {
            gameInterval = setInterval(() => {
                if (!isPlaying) return;
                let containerWidth = gameContainer.offsetWidth;
                blockLeft -= gameSpeed;
                if (blockLeft < -20) {
                    blockLeft = containerWidth;
                    score += 10;
                    scoreElem.innerText = score;
                    if (score % 40 === 0) gameSpeed += 0.5;
                }
                block.style.left = blockLeft + "px";
                let heroBottom = parseInt(window.getComputedStyle(hero).getPropertyValue("bottom"));
                if (blockLeft > 30 && blockLeft < 60 && heroBottom < 28) {
                    isPlaying = false;
                    clearInterval(gameInterval);
                    if (score > highScore) {
                        highScore = score;
                        localStorage.setItem("mk_desktop_high", highScore);
                        highScoreElem.innerText = highScore;
                    }
                    gameOverScreen.style.display = "flex";
                }
            }, 20);
        }

        function triggerRestart(e) {
            if(e) e.stopPropagation();
            blockLeft = gameContainer.offsetWidth;
            score = 0;
            gameSpeed = 6;
            scoreElem.innerText = score;
            gameOverScreen.style.display = "none";
            block.style.left = blockLeft + "px";
            isPlaying = true;
            startGameLoop();
        }
        restartBtn.addEventListener("touchend", (e) => { e.preventDefault(); triggerRestart(); });
        restartBtn.addEventListener("click", triggerRestart);
        startGameLoop();

        /* 2: Memory Match */
        const emojis = ['🚀', '💻', '🎮', '🔥', '🚀', '💻', '🎮', '🔥'];
        let memoryGrid = document.getElementById("memory-grid");
        let firstCard = null, lockBoard = false, matchesFound = 0;

        function initMemoryGame() {
            memoryGrid.innerHTML = "";
            matchesFound = 0; firstCard = null; lockBoard = false;
            document.getElementById("mem-status").innerText = "طابق الرموز المتشابهة";
            let shuffled = emojis.sort(() => 0.5 - Math.random());
            shuffled.forEach(emoji => {
                let card = document.createElement("div");
                card.classList.add("memory-card");
                card.dataset.emoji = emoji;
                card.addEventListener("click", flipCard);
                card.addEventListener("touchend", (e) => { e.preventDefault(); flipCard.call(card); });
                memoryGrid.appendChild(card);
            });
        }
        function flipCard() {
            if (lockBoard || this === firstCard || this.classList.contains("flipped")) return;
            this.classList.add("flipped");
            this.innerText = this.dataset.emoji;
            if (!firstCard) { firstCard = this; return; }
            let secondCard = this;
            if (firstCard.dataset.emoji === secondCard.dataset.emoji) {
                firstCard = null; matchesFound += 2;
                if (matchesFound === emojis.length) document.getElementById("mem-status").innerText = "🎉 فزت في اللعبة!";
            } else {
                lockBoard = true;
                setTimeout(() => {
                    firstCard.classList.remove("flipped"); firstCard.innerText = "";
                    secondCard.classList.remove("flipped"); secondCard.innerText = "";
                    firstCard = null; lockBoard = false;
                }, 700); 
