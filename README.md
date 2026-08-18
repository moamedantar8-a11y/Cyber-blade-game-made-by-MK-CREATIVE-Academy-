<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
    <title>MK CREATIVE Arcade Hub</title>
    <style>
        body {
            background-color: #030712;
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
        }

        h1.main-title {
            color: #00ffcc;
            font-size: 1.5rem;
            margin: 5px 0 15px 0;
            text-shadow: 0 0 15px rgba(0,255,204,0.5);
        }

        /* أزرار التنقل بين الألعاب */
        .arcade-tabs {
            display: flex;
            gap: 10px;
            margin-bottom: 15px;
        }
        .tab-btn {
            padding: 8px 16px;
            background: #1e293b;
            color: #94a3b8;
            border: 1px solid #334155;
            border-radius: 6px;
            font-weight: bold;
            font-size: 0.9rem;
            cursor: pointer;
        }
        .tab-btn.active {
            background: #00ffcc;
            color: #030712;
            border-color: #00ffcc;
            box-shadow: 0 0 10px rgba(0,255,204,0.4);
        }

        /* حاوية الألعاب */
        .game-section {
            display: none;
            width: 90vw;
            max-width: 400px;
            flex-direction: column;
            align-items: center;
        }
        .game-section.active {
            display: flex;
        }

        /* لعبة 1: Cyber Blade */
        #game {
            width: 100%;
            height: 190px;
            border: 2px solid #00ffcc;
            position: relative;
            background: #0f172a;
            overflow: hidden;
            border-radius: 10px;
            box-shadow: 0 0 20px rgba(0,255,204,0.2);
        }
        #hero {
            width: 28px;
            height: 28px;
            background: #00ffcc;
            position: absolute;
            bottom: 0;
            left: 40px;
            border-radius: 4px;
        }
        #block {
            width: 20px;
            height: 38px;
            background: #ff3366;
            position: absolute;
            bottom: 0;
            left: 400px;
            border-radius: 4px;
        }
        .jump {
            animation: heroJump 0.4s cubic-bezier(0,0,0.2,1);
        }
        @keyframes heroJump {
            0% { bottom: 0; }
            50% { bottom: 95px; }
            100% { bottom: 0; }
        }
        #score-board {
            width: 100%;
            display: flex;
            justify-content: space-between;
            margin-bottom: 8px;
            font-size: 1rem;
            font-weight: bold;
            color: #cbd5e1;
        }
        #score-board span { color: #00ffcc; }
        
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
        #game-over h2 { color: #ff3366; font-size: 1.6rem; margin-bottom: 12px; }
        
        /* زر إعادة المحاولة المصحح للموبايل */
        .restart-btn {
            padding: 12px 28px;
            background: #00ffcc;
            color: #030712;
            border: none;
            font-weight: bold;
            border-radius: 8px;
            font-size: 1rem;
            cursor: pointer;
            box-shadow: 0 0 15px rgba(0,255,204,0.6);
            z-index: 30;
            -webkit-tap-highlight-color: transparent;
        }

        /* لعبة 2: تحدي الذاكرة (Memory Game) */
        .memory-grid {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 8px;
            width: 100%;
            margin-bottom: 15px;
        }
        .memory-card {
            aspect-ratio: 1;
            background: #1e293b;
            border: 2px solid #334155;
            border-radius: 8px;
            font-size: 1.5rem;
            display: flex;
            justify-content: center;
            align-items: center;
            cursor: pointer;
            user-select: none;
        }
        .memory-card.flipped {
            background: #0f172a;
            border-color: #00ffcc;
        }

        .hint {
            margin-top: 10px;
            color: #94a3b8;
            font-size: 0.85rem;
        }
        .copyrights {
            margin-top: 25px;
            color: #64748b;
            font-size: 0.8rem;
        }
        .copyrights span { color: #00ffcc; font-weight: bold; }
    </style>
</head>
<body>

    <h1 class="main-title">MK CREATIVE Arcade</h1>

    <!-- أزرار الاختيار بين اللعبتين -->
    <div class="arcade-tabs">
        <button class="tab-btn active" onclick="switchGame(1)">🎮 Cyber Jump</button>
        <button class="tab-btn" onclick="switchGame(2)">🧠 Memory Match</button>
    </div>

    <!-- اللعبة الأولى: القفز العنيف -->
    <div id="section-1" class="game-section active">
        <div id="score-board">
            <div>النقاط: <span id="score">0</span></div>
            <div>الأعلى: <span id="high-score">0</span></div>
        </div>
        <div id="game">
            <div id="hero"></div>
            <div id="block"></div>
            
            <div id="game-over">
                <h2>انتهت اللعبة</h2>
                <button class="restart-btn" id="restart-btn">إعادة المحاولة</button>
            </div>
        </div>
        <div class="hint">اضغط بصباعك على اللعبة للقفز! 👆</div>
    </div>

    <!-- اللعبة الثانية: الذاكرة -->
    <div id="section-2" class="game-section">
        <div id="score-board">
            <div>الحالة: <span id="mem-status">طابق الرموز المتشابهة</span></div>
        </div>
        <div class="memory-grid" id="memory-grid"></div>
        <button class="restart-btn" onclick="initMemoryGame()">لعبة جديدة</button>
        <div class="hint">اضغط على المربعات لاكتشاف الرموز! 🧠</div>
    </div>

    <div class="copyrights">
        © 2026 <span>MK CREATIVE Agency</span>. All Rights Reserved.
    </div>

    <script>
        /* نظام التبديل بين الألعاب */
        function switchGame(gameNum) {
            document.querySelectorAll('.game-section').forEach(sec => sec.classList.remove('active'));
            document.querySelectorAll('.tab-btn').forEach(btn => btn.classList.remove('active'));
            
            document.getElementById(`section-${gameNum}`).classList.add('active');
            event.target.classList.add('active');
            
            if(gameNum === 1 && !isPlaying) {
                // إعادة تهيئة السيرش لو رجع للعبة الأولى
            }
        }

        /* برمجة اللعبة الأولى (Cyber Jump) */
        const hero = document.getElementById("hero");
        const block = document.getElementById("block");
        const scoreElem = document.getElementById("score");
        const highScoreElem = document.getElementById("high-score");
        const gameOverScreen = document.getElementById("game-over");
        const gameContainer = document.getElementById("game");
        const restartBtn = document.getElementById("restart-btn");

        let score = 0;
        let highScore = localStorage.getItem("mk_mobile_high") || 0;
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

        // تفاعل باللمس المباشر بدون مشاكل للموبايل
        gameContainer.addEventListener("touchstart", (e) => {
            e.preventDefault();
            jump();
        }, {passive: false});

        gameContainer.addEventListener("click", () => {
            jump();
        });

        function startGameLoop() {
            gameInterval = setInterval(() => {
                if (!isPlaying) return;

                blockLeft -= gameSpeed;
                if (blockLeft < -20) {
                    blockLeft = 400;
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
                        localStorage.setItem("mk_mobile_high", highScore);
                        highScoreElem.innerText = highScore;
                    }

                    gameOverScreen.style.display = "flex";
                }
            }, 20);
        }

        function triggerRestart(e) {
            if(e) e.stopPropagation();
            blockLeft = 400;
            score = 0;
            gameSpeed = 6;
            scoreElem.innerText = score;
            gameOverScreen.style.display = "none";
            block.style.left = blockLeft + "px";
            isPlaying = true;
            startGameLoop();
        }

        // دعم لمس زر إعادة المحاولة بشكل مؤكد للموبايل
        restartBtn.addEventListener("touchend", (e) => {
            e.preventDefault();
            triggerRestart();
        });
        restartBtn.addEventListener("click", triggerRestart);

        // بدء اللعبة الأولى فوراً
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
            checkForMatch(firstCard, secondCard);
        }

        function checkForMatch(card1, card2) {
            let isMatch = card1.dataset.emoji === card2.dataset.emoji;

            if (isMatch) {
                card1.removeEventListener("click", flipCard);
                card2.removeEventListener("click", flipCard);
                resetTurn();
                matchesFound += 2;
                if (matchesFound === emojis.length) {
                    document.getElementById("mem-status").innerText = "🎉 فزت في اللعبة!";
                }
            } else {
                lockBoard = true;
                setTimeout(() => {
                    card1.classList.remove("flipped");
                    card1.innerText = "";
                    card2.classList.remove("flipped");
                    card2.innerText = "";
                    resetTurn();
                }, 800);
            }
        }

        function resetTurn() {
            firstCard = null;
            lockBoard = false;
        }

        initMemoryGame();
    </script>
</body>
</html> 
