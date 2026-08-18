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

        .top-bar {
            display: flex;
            justify-content: space-between;
            align-items: center;
            width: 90vw;
            max-width: 650px;
            margin-bottom: 10px;
        }

        h1.main-title {
            color: var(--accent);
            font-size: 1.4rem;
            margin: 0;
            text-shadow: 0 0 15px rgba(0,255,204,0.5);
        }

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

        /* أزرار التنقل بين الألعاب الـ 6 */
        .arcade-tabs {
            display: flex;
            gap: 6px;
            margin-bottom: 15px;
            flex-wrap: wrap;
            justify-content: center;
            max-width: 650px;
        }
        .tab-btn {
            padding: 6px 10px;
            background: #1e293b;
            color: #94a3b8;
            border: 1px solid #334155;
            border-radius: 6px;
            font-weight: bold;
            font-size: 0.75rem;
            cursor: pointer;
        }
        .tab-btn.active {
            background: var(--accent);
            color: #030712;
            border-color: var(--accent);
            box-shadow: 0 0 12px rgba(0,255,204,0.5);
        }

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

        /* لعبة 2: الذاكرة */
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

        /* كروت الألعاب الفرعية وتنسيقاتها */
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

        /* شبكة لعبة إكس أوت (Tic Tac Toe) */
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

    <!-- أزرار التنقل بين الألعاب الـ 6 -->
    <div class="arcade-tabs">
        <button class="tab-btn active" onclick="switchGame(1)">🎮 Jump</button>
        <button class="tab-btn" onclick="switchGame(2)">🧠 Memory</button>
        <button class="tab-btn" onclick="switchGame(3)">🔢 Guess</button>
        <button class="tab-btn" onclick="switchGame(4)">✂️ RPS</button>
        <button class="tab-btn" onclick="switchGame(5)">❌ TicTacToe</button>
        <button class="tab-btn" onclick="switchGame(6)">🎲 Dice</button>
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
        <div class="hint">اضغط مسافة أو انقر بالشاشة للقفز! 👆</div>
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

    <!-- اللعبة الثالثة: Guess The Number -->
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

    <!-- اللعبة الرابعة: Rock Paper Scissors (تم ضبط زر الصخرة 🪨) -->
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

    <!-- اللعبة الخامسة الجديدة: Tic Tac Toe -->
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

    <!-- اللعبة السادسة الجديدة: Dice Roll Challenge -->
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


        /* برمجة اللعبة الثالثة (Guess The Number) */
        let targetNum = 0;
        function initGuessGame() {
            targetNum = Math.floor(Math.random() * 50) + 1;
            document.getElementById("guess-status").innerText = "اختر رقماً من 1 إلى 50";
            document.getElementById("guess-feedback").innerText = "";
            document.getElementById("guess-input").value = "";
        }

        function checkGuess() {
            let userVal = parseInt(document.getElementById("guess-input").value);
            let feedback = document.getElementById("guess-feedback");
            
            if(isNaN(userVal)) {
                feedback.innerText = "الرجاء إدخال رقم صحيح!";
                return;
            }

            if(userVal === targetNum) {
                feedback.innerText = "🎉 بطل! لقد اخترت الرقم الصحيح!";
                document.getElementById("guess-status").innerText = "فزت في التحدي!";
            } else if(userVal < targetNum) {
                feedback.innerText = "⬆️ الرقم السحري أعلى من كده!";
            } else {
                feedback.innerText = "⬇️ الرقم السحري أقل من كده!";
            }
        }
        initGuessGame();


        /* برمجة اللعبة الرابعة (Rock Paper Scissors) مع ضبط أيقونة الصخرة 🪨 */
        let pScore = 0, aScore = 0;
        const rpsIcons = { rock: '🥌', paper: '📄', scissors: '✂️' };

        function playRPS(playerChoice) {
            const choices = ['rock', 'paper', 'scissors'];
            const aiChoice = choices[Math.floor(Math.random() * 3)];
            let resultText = "";

            if (playerChoice === aiChoice) {
                resultText = "🤝 تعادل!";
            } else if (
                (playerChoice === 'rock' && aiChoice === 'scissors') ||
                (playerChoice === 'paper' && aiChoice === 'rock') ||
                (playerChoice === 'scissors' && aiChoice === 'paper')
            ) {
                resultText = "🎉 لقد فزت بهذه الجولة!";
                pScore++;
            } else {
                resultText = "❌ فاز الذكاء الاصطناعي!";
                aScore++;
            }

            document.getElementById("player-score").innerText = pScore;
            document.getElementById("ai-score").innerText = aScore;
            document.getElementById("rps-result-text").innerText = resultText;
            document.getElementById("rps-details").innerText = `أنت اخترت ${rpsIcons[playerChoice]} | الكمبيوتر اختار ${rpsIcons[aiChoice]}`;
        }


        /* برمجة اللعبة الخامسة الجديدة (Tic Tac Toe) */
        let tttBoard = ['', '', '', '', '', '', '', '', ''];
        let tttGameActive = true;
        let tttPlayer = 'X';

        function makeMove(index) {
            if (tttBoard[index] === '' && tttGameActive) {
                tttBoard[index] = tttPlayer;
                document.getElementsByClassName('ttt-cell')[index].innerText = tttPlayer;
                
                checkWinnerTTT();
                
                if (tttGameActive) {
                    tttPlayer = 'O';
                    document.getElementById('ttt-status').innerText = "دور الكمبيوتر (O)";
                    setTimeout(aiMoveTTT, 400);
                }
            }
        }

        function aiMoveTTT() {
            if (!tttGameActive) return;
            let emptyCells = [];
            tttBoard.forEach((val, idx) => { if(val === '') emptyCells.push(idx); });
            
            if (emptyCells.length > 0) {
                let randomIndex = emptyCells[Math.floor(Math.random() * emptyCells.length)];
                tttBoard[randomIndex] = 'O';
                document.getElementsByClassName('ttt-cell')[randomIndex].innerText = 'O';
                checkWinnerTTT();
                if (tttGameActive) {
                    tttPlayer = 'X';
                    document.getElementById('ttt-status').innerText = "دورك (X)";
                }
            }
        }

        function checkWinnerTTT() {
            const winConditions = [
                [0,1,2], [3,4,5], [6,7,8],
                [0,3,6], [1,4,7], [2,5,8],
                [0,4,8], [2,4,6]
            ];
            let roundWon = false;
            
            for (let condition of winConditions) {
                let a = tttBoard[condition[0]], b = tttBoard[condition[1]], c = tttBoard[condition[2]];
                if (a === '' || b === '' || c === '') continue;
                if (a === b && b === c) { roundWon = true; break; }
            }

            if (roundWon) {
                document.getElementById('ttt-status').innerText = `🎉 الفائز هو ${tttPlayer}!`;
                tttGameActive = false;
                return;
            }

            if (!tttBoard.includes('')) {
                document.getElementById('ttt-status').innerText = "🤝 تعادل!";
                tttGameActive = false;
            }
        }

        function resetTTT() {
            tttBoard = ['', '', '', '', '', '', '', '', ''];
            tttGameActive = true;
            tttPlayer = 'X';
            document.getElementById('ttt-status').innerText = "دورك (X)";
            Array.from(document.getElementsByClassName('ttt-cell')).forEach(cell => cell.innerText = '');
        }


        /* برمجة اللعبة السادسة الجديدة (Dice Roll Challenge) */
        let dicePScore = 0, diceCScore = 0;
        const diceFaces = ['⚀', '⚁', '⚂', '⚃', '⚄', '⚅'];

        function rollDice() {
            let pRoll = Math.floor(Math.random() * 6) + 1;
            let cRoll = Math.floor(Math.random() * 6) + 1;
            
            document.getElementById('dice-display').innerText = `${diceFaces[pRoll-1]} vs ${diceFaces[cRoll-1]}`;
            
            if (pRoll > cRoll) {
                dicePScore++;
                document.getElementById('dice-msg').innerText = `🎉 فزت في الرمية (${pRoll} مقابل ${cRoll})`;
            } else if (pRoll < cRoll) {
                diceCScore++;
                document.getElementById('dice-msg').innerText = `❌ فاز الكمبيوتر (${cRoll} مقابل ${pRoll})`;
            } else {
                document.getElementById('dice-msg').innerText = `🤝 تعادل في الرمية (${pRoll})`;
            }
            
            document.getElementById('dice-p').innerText = dicePScore;
            document.getElementById('dice-c').innerText = diceCScore;
        }
    </script>
</body>
</html> 
