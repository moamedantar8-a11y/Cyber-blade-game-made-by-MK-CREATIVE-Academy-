<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Cyber Blade: MK Agency Escape</title>
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
            animation: hideSplash 0.5s ease 2.5s forwards;
        }
        @keyframes hideSplash {
            to { opacity: 0; visibility: hidden; }
        }
        #splash h1 { color: #00ffcc; font-size: 2.5rem; text-shadow: 0 0 15px rgba(0,255,204,0.6); }
        #splash p { color: #94a3b8; margin-top: 10px; }

        /* منطقة اللعبة */
        #game {
            width: 700px;
            height: 220px;
            border: 2px solid #00ffcc;
            position: relative;
            background: #0f172a;
            overflow: hidden;
            border-radius: 8px;
            box-shadow: 0 0 20px rgba(0,255,204,0.2);
        }
        #hero {
            width: 30px;
            height: 30px;
            background: #00ffcc;
            position: absolute;
            bottom: 0;
            left: 50px;
            border-radius: 4px;
        }
        #block {
            width: 20px;
            height: 40px;
            background: #ff3366;
            position: absolute;
            bottom: 0;
            left: 700px;
            border-radius: 4px;
        }
        .jump {
            animation: heroJump 0.4s linear;
        }
        @keyframes heroJump {
            0% { bottom: 0; }
            50% { bottom: 90px; }
            100% { bottom: 0; }
        }
        #score-board {
            font-size: 1.2rem;
            margin-bottom: 10px;
            font-weight: bold;
        }
        #game-over {
            position: absolute;
            top: 0; left: 0; width: 100%; height: 100%;
            background: rgba(0,0,0,0.85);
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            display: none;
            z-index: 10;
        }
        #game-over h2 { color: #ff3366; font-size: 2rem; margin-bottom: 10px; }
        #restart-btn {
            padding: 8px 20px;
            background: #00ffcc;
            color: #000;
            border: none;
            font-weight: bold;
            border-radius: 4px;
            cursor: pointer;
        }
    </style>
</head>
<body>

    <!-- شاشة البداية للوكالة -->
    <div id="splash">
        <h1>Made by MK CREATIVE Agency</h1>
        <p>Loading Game...</p>
    </div>

    <div id="score-board">Score: <span id="score">0</span></div>

    <div id="game">
        <div id="hero"></div>
        <div id="block"></div>
        
        <div id="game-over">
            <h2>GAME OVER</h2>
            <button id="restart-btn" onclick="restartGame()">Play Again</button>
        </div>
    </div>

    <p style="color: #94a3b8; margin-top: 15px;">اضغط مسافة (Space) للقفز!</p>

    <script>
        const hero = document.getElementById("hero");
        const block = document.getElementById("block");
        const scoreElem = document.getElementById("score");
        const gameOverScreen = document.getElementById("game-over");

        let score = 0;
        let isPlaying = false;
        let blockLeft = 700;
        let gameInterval;

        // البدء بعد شاشة البداية
        setTimeout(() => {
            isPlaying = true;
            startGameLoop();
        }, 2500);

        function jump() {
            if (!hero.classList.contains("jump") && isPlaying) {
                hero.classList.add("jump");
                setTimeout(() => {
                    hero.classList.remove("jump");
                }, 400);
            }
        }

        document.addEventListener("keydown", (e) => {
            if (e.code === "Space") {
                jump();
                e.preventDefault();
            }
        });

        document.addEventListener("click", () => {
            if(isPlaying) jump();
        });

        function startGameLoop() {
            gameInterval = setInterval(() => {
                if (!isPlaying) return;

                blockLeft -= 7;
                if (blockLeft < -20) {
                    blockLeft = 700;
                    score += 10;
                    scoreElem.innerText = score;
                }
                block.style.left = blockLeft + "px";

                // فحص الاصطدام
                let heroBottom = parseInt(window.getComputedStyle(hero).getPropertyValue("bottom"));

                if (blockLeft > 40 && blockLeft < 70 && heroBottom < 30) {
                    isPlaying = false;
                    clearInterval(gameInterval);
                    gameOverScreen.style.display = "flex";
                }
            }, 20);
        }

        function restartGame() {
            blockLeft = 700;
            score = 0;
            scoreElem.innerText = score;
            gameOverScreen.style.display = "none";
            block.style.left = blockLeft + "px";
            isPlaying = true;
            startGameLoop();
        }
    </script>
</body>
</html>08
