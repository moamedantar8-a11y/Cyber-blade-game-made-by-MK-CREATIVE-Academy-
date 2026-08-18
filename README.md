<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
    <title>MK CREATIVE Agency - Pro Arcade Hub</title>
    <style>
        :root {
            --bg-color: #030712;
            --container-width: 400px;
            --game-height: 200px;
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
            padding: 10px 0;
            touch-action: manipulation;
            transition: background 0.3s;
        }

        /* رأس الصفحة وأزرار التحكم */
        .top-bar {
            display: flex;
            justify-content: space-between;
            align-items: center;
            width: 90vw;
            max-width: 600px;
            margin-bottom: 10px;
        }

        h1.main-title {
            color: var(--accent);
            font-size: 1.4rem;
            margin: 0;
            text-shadow: 0 0 15px rgba(0,255,204,0.5);
        }

        /* زر وضع الكمبيوتر / الموبايل */
        .mode-toggle-btn {
            background: #1e293b;
            color: var(--accent);
            border: 1px solid var(--accent);
            padding: 6px 12px;
            border-radius: 6px;
            font-size: 0.8rem;
            font-weight: bold;
            cursor: pointer;
            box-shadow: 0 0 8px rgba(0,255,204,0.3);
        }

        /* أزرار التنقل بين الألعاب */
        .arcade-tabs {
            display: flex;
            gap: 8px;
            margin-bottom: 15px;
            flex-wrap: wrap;
            justify-content: center;
        }
        .tab-btn {
            padding: 7px 14px;
            background: #1e293b;
            color: #94a3b8;
            border: 1px solid #334155;
            border-radius: 6px;
            font-weight: bold;
            font-size: 0.85rem;
            cursor: pointer;
        }
        .tab-btn.active {
            background: var(--accent);
            color: #030712;
            border-color: var(--accent);
            box-shadow: 0 0 12px rgba(0,255,204,0.5);
        }

        /* حاوية الألعاب المتغيرة الحجم */
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

        /* لوحة النتائج */
        .score-board {
            width: 100%;
            display: flex;
            justify-content: space-between;
            margin-bottom: 8px;
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
            width: 28px;
            height: 28px;
            background: var(--accent);
            position: absolute;
            bottom: 0;
            left: 40px;
            border-radius: 4px;
            box-shadow: 0 0 10px var(--accent);
        }
        #block {
            width: 20px;
            height: 38px;
            background: #ff3366;
            position: absolute;
            bottom: 0;
            left: 100%;
            border-radius: 4px;
            box-shadow: 0 0 10px #ff3366;
        }
        .jump {
            animation: heroJump 0.4s cubic-bezier(0,0,0.2,1);
        }
        @keyframes heroJump {
            0% { bottom: 0; }
            50% { bottom: 100px; }
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

        /* لعبة 2: تحدي الذاكرة */
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

        /* لعبة 3: سرعة النقر (Clicker Challenge) */
        .clicker-box {
            width: 100%;
            height: 180px;
            background: #1e293b;
            border: 2px dashed var(--accent);
            border-radius: 10px;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            cursor: pointer;
            user-select: none;
            box-shadow: inset 0 0 15px rgba(0,255,204,0.1);
        }
        .clicker-box h2 { font-size: 2.2rem; margin: 0; color: var(--accent); }
        .clicker-box p { color: #94a3b8; font-size: 0.85rem; margin-top: 5px; }

        .hint {
            margin-top: 10px;
            color: #94a3b8;
            font-size: 0.8rem;
        }
        .copyrights {
            margin-top: 20px;
            color: #64748b;
            font-size: 0.75rem;
        }
        .copyrights span { color: var(--accent); font-weight: bold; }
    </style>
</head>
<body>

    <div class="top-bar">
        <h1 class="main-title">MK Arcade Hub</h1>
        <button class="mode-toggle-btn" onclick="toggleDesktopMode()" id="mode-btn">💻 وضع الكمبيوتر: متوقف</button>
    </div>

    <!-- أزرار التنقل بين الألعاب -->
    <div class="arcade-tabs">
        <button class="tab-btn active" onclick="switchGame(1)">🎮 Cyber Jump</button>
        <button class="tab-btn" onclick="switchGame(2)">🧠 Memory Match</button>
        <button class="tab-btn" onclick="switchGame(3)">⚡ Speed Clicker</button>
    </div>

    <!-- اللعبة الأولى: Cyber Jump -->
    <div id="section-1" class="game-section active">
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
        <div class="hint">اضغط مسافة (كيبورد) أو انقر بالشاشة للقفز! 👆</div>
    </div>

    <!-- اللعبة الثانية: Memory Match -->
    <div id="section-2" class="game-section">
        <div class="score-board">
            <div>الحالة: <span id="mem-status">طابق الرموز المتشابهة</span></div>
        </div>
        <div class="memory-grid" id="memory-grid"></div>
        <button class="action-btn" onclick="initMemoryGame()">لعبة جديدة</button>
        <div class="hint">اكتشف كروت الرموز المتطابقة! 🧠</div>
    </div>

    <!-- اللعبة الثالثة: Speed Clicker -->
    <div id="section-3" class="game-section">
        <div class="score-board">
            <div>الوقت المتبقي: <span id="time-left">5</span> ثواني</div>
        </div>
        <div class="clicker-box" id="clicker-target">
            <h2 id="click-count">0</h2>
            <p id="clicker-msg">اضغط هنا بأسرع ما يمكنك لبدء التحدي!</p>
        </div>
        <button class="action-btn" style="margin-top: 10px;" onclick="resetClicker()">إعادة التحدي</button>
        <div class="hint">اضغط بأسرع سرعة قبل انتهاء العداد! ⚡</div>
    </div>

    <div class="copyrights">
        © 2026 <span>MK CREATIVE Agency</span>. All Rights Reserved.
    </div>

    <script>
        /* نظام وضع الكمبيوتر / الموبايل */
        let isDesktop = false;
        function toggleDesktopMode() {
            isDesktop = !isDesktop;
            const root = document.documentElement;
            const modeBtn = document.getElementById("mode-btn");

            if (isDesktop) {
                root.style.setProperty('--container-width', '650px');
                root.style.setProperty('--game-height', '260px');
                modeBtn.innerText = "💻 وضع الكمبيوتر: مُفعّل";
                modeBtn.style.background = "var(--accent)";
                modeBtn.style.color = "#030712";
            } else {
                root.style.setProperty('--container-width', '400px');
                root.style.setProperty('--game-height', '200px');
                modeBtn.innerText = "💻 وضع الكمبيوتر: متوقف";
                modeBtn.style.background = "#1e293b";
                modeBtn.style.color = "var(--accent)";
            }
        }

        /* نظام التبديل بين الألعاب */
        function switchGame(gameNum) {
            document.querySelectorAll('.game-section').forEach(sec => sec.classList.remove('active'));
            document.querySelectorAll('.tab-btn').forEach(btn => btn.classList.remove('active'));
            
            document.getElementById(`section-${gameNum}`).classList.add('active');
            event.target.classList.add('active');
        }

        /* برمجة اللعبة الأولى (Cyber Jump) */
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
                setTimeout(() => {
                    hero.classList.remove("jump");
                }, 400);
            }
        }

        gameContainer.addEventListener("touchstart", (e) => {
            e.preventDefault();
            jump();
        }, {passive: false});

        gameContainer.addEventListener("click", () => {
            jump();
        });

        document.addEventListener("keydown", (e) => {
            if (e.code === "Space") {
                jump();
                e.preventDefault();
            }
        });

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

        restartBtn.addEventListener("touchend", (e) => {
            e.preventDefault();
            triggerRestart();
        });
        restartBtn.addEventListener("click", triggerRestart);

        startGameLoop();


        /* برمجة اللعبة الثانية (Memory Game) */
        const emojis = ['🚀', '💻', '🎮', '🔥', '🚀', '💻', '🎮', '🔥'];
        let memoryGrid = document.getElementById("memory-grid");
        let firstCard = null;
        let lockBoard = false;
        let matchesFound = 0;

        function initMemoryGame() {
            memoryGrid.innerHTML = "";
            matchesFound = 0;
            firstCard = null;
            lockBoard = false;
            document.getElementById("mem-status").innerText = "طابق الرموز المتشابهة";
            
            let shuffled = emojis.sort(() => 0.5 - Math.random());
            
            shuffled.forEach(emoji => {
                let card = document.createElement("div");
                card.classList.add("memory-card");
                card.dataset.emoji = emoji;
                
                card.addEventListener("click", flipCard);
                card.addEventListener("touchend", (e) => {
                    e.preventDefault();
                    flipCard.call(card);
                });
                
                memoryGrid.appendChild(card);
            });
        }

        function flipCard() {
            if (lockBoard) return;
            if (this === firstCard) return;
            if (this.classList.contains("flipped")) return;

            this.classList.add("flipped");
            this.innerText = this.dataset.emoji;

            if (!firstCard) {
                firstCard = this;
                return;
            }

            let secondCard = this;
            let isMatch = firstCard.dataset.emoji === secondCard.dataset.emoji;

            if (isMatch) {
                firstCard = null;
                matchesFound += 2;
                if (matchesFound === emojis.length) {
                    document.getElementById("mem-status").innerText = "🎉 فزت في اللعبة!";
                }
            } else {
                lockBoard = true;
                setTimeout(() => {
                    firstCard.classList.remove("flipped");
                    firstCard.innerText = "";
                    secondCard.classList.remove("flipped");
                    secondCard.innerText = "";
                    firstCard = null;
                    lockBoard = false;
                }, 700);
            }
        }
        initMemoryGame();


        /* برمجة اللعبة الثالثة (Speed Clicker) */
        let clickCount = 0;
        let timeLeft = 5;
        let clickerActive = false;
        let clickTimer;
        
        const clickerBox = document.getElementById("clicker-target");
        const clickCountElem = document.getElementById("click-count");
        const timeLeftElem = document.getElementById("time-left");
        const clickerMsg = document.getElementById("clicker-msg");

        function handleClicks() {
            if (!clickerActive && timeLeft === 5) {
                clickerActive = true;
                clickerMsg.innerText = "اضغط بأقصى سرعة!";
                clickTimer = setInterval(() => {
                    timeLeft--;
                    timeLeftElem.innerText = timeLeft;
                    if (timeLeft <= 0) {
                        clearInterval(clickerTimer);
                        clickerActive = false;
                        clickerMsg.innerText = `انتهى الوقت! النتيجة: ${clickCount} نقرة 🔥`;
                    }
                }, 1000);
            }

            if (clickerActive) {
                clickCount++;
                clickCountElem.innerText = clickCount;
            }
        }

        clickerBox.addEventListener("click", handleClicks);
        clickerBox.addEventListener("touchstart", (e) => {
            e.preventDefault();
            handleClicks();
        });

        function resetClicker() {
            clearInterval(clickTimer);
            clickerActive = false;
            clickCount = 0;
            timeLeft = 5;
            clickCountElem.innerText = "0";
            timeLeftElem.innerText = "5";
            clickerMsg.innerText = "اضغط هنا بأسرع ما يمكنك لبدء التحدي!";
        }
    </script>
</body>
</html> 
