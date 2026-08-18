<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
    <title>Cyber Blade: MK Escape</title>
    <style>
        body {
            background-color: #030712;
            color: #fff;
            text-align: center;
            font-family: 'Segoe UI', Tahoma, sans-serif;
            margin: 0;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            height: 100vh;
            overflow: hidden;
            touch-action: manipulation;
        }

        /* شاشة البداية للوكالة */
        #splash {
            position: fixed;
            top: 0; left: 0; width: 100%; height: 100%;
            background: #000;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            z-index: 99;
            animation: hideSplash 0.5s ease 2s forwards;
        }
        @keyframes hideSplash {
            to { opacity: 0; visibility: hidden; }
        }
        #splash h1 { color: #00ffcc; font-size: 1.8rem; text-shadow: 0 0 15px rgba(0,255,204,0.6); padding: 0 10px; }
        #splash p { color: #94a3b8; margin-top: 10px; font-size: 1rem; }

        /* حاوية اللعبة */
        #game {
            width: 90vw;
            max-width: 400px;
            height: 200px;
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
            box-shadow: 0 0 10px #00ffcc;
        }
        #block {
            width: 20px;
            height: 38px;
            background: #ff3366;
            position: absolute;
            bottom: 0;
            left: 400px;
            border-radius: 4px;
            box-shadow: 0 0 10px #ff3366;
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
            width: 90vw;
            max-width: 400px;
            display: flex;
            justify-content: space-between;
            margin-bottom: 10px;
            font-size: 1.1rem;
            font-weight: bold;
            color: #cbd5e1;
        }
        #score-board span { color: #00ffcc; }
        
        #game-over {
            position: absolute;
            top: 0; left: 0; width: 100%; height: 100%;
            background: rgba(3, 7, 18, 0.9);
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            display: none;
            z-index: 10;
        }
        #game-over h2 { color: #ff3366; font-size: 1.8rem; margin-bottom: 8px; }
        #restart-btn {
            padding: 10px 22px;
            background: #00ffcc;
            color: #030712;
            border: none;
            font-weight: bold;
            border-radius: 6px;
            cursor: pointer;
            font-size: 1rem;
            box-shadow: 0 0 12px rgba(0,255,204,0.4);
        }

        /* حقوق الملكية الفكرية بتصميم فخم أسفل اللعبة */
        .copyrights {
            margin-top: 15px;
            color: #64748b;
            font-size: 0.85rem;
            letter-spacing: 0.5px;
        }
        .copyrights span {
            color: #00ffcc;
            font-weight: bold;
        }
        .hint {
            margin-top: 8px;
            color: #94a3b8;
            font-size: 0.85rem;
        }
    </style>
</head>
<body>

    <div id="splash">
        <h1>MK CREATIVE Agency</h1>
        <p>Loading Game...</p>
    </div>

    <div id="score-board">
        <div>النقاط: <span id="score">0</span></div>
        <div>الأعلى: <span id="high-score">0</span></div>
    </div>

    <div id="game">
        <div id="hero"></div>
        <div id="block"></div>
        
        <div id="game-over">
            <h2>انتهت اللعبة</h2>
            <button id="restart-btn" onclick="restartGame()">إعادة المحاولة</button>
        </div>
    </div>

    <div class="hint">اضغط بصباعك على الشاشة للقفز! 👆</div>
    
    <!-- حقوق الملكية الفكرية -->
    <div class="copyrights">
        © 2026 <span>MK CREATIVE Agency</span>. All Rights Reserved.
    </div>

    <script>
        const hero = document.getElementById("hero");
        const block = document.getElementById("block");
        const scoreElem = document.getElementById("score");
        const highScoreElem = document.getElementById("high-score");
        const gameOverScreen = document.getElementById("game-over");
        const gameContainer = document.getElementById("game");

        let score = 0;
        let highScore = localStorage.getItem("mk_mobile_high") || 0;
        highScoreElem.innerText = highScore;

        let isPlaying = false;
        let blockLeft = 400;
        let gameSpeed = 6;
        let gameInterval;

        setTimeout(() => {
            isPlaying = true;
            startGameLoop();
        }, 2000);

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
        });

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

                blockLeft -= gameSpeed;
                if (blockLeft < -20) {
                    blockLeft = 400;
                    score += 10;
                    scoreElem.innerText = score;
                    
                    if (score % 40 === 0) {
                        gameSpeed += 0.5;
                    }
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

        function restartGame() {
            blockLeft = 400;
            score = 0;
            gameSpeed = 6;
            scoreElem.innerText = score;
            gameOverScreen.style.display = "none";
            block.style.left = blockLeft + "px";
            isPlaying = true;
            startGameLoop();
        }
    </script>
</body>
</html>
