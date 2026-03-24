<!DOCTYPE html>
<html>
<head>
    <style>
        body { font-family: sans-serif; text-align: center; background: #1a1a2e; color: white; padding: 20px; }
        #game-box { border: 3px solid #4ecca3; border-radius: 20px; min-height: 300px; padding: 20px; background: #16213e; }
        .hidden { display: none; }
        button { background: #4ecca3; color: #1a1a2e; border: none; padding: 12px 24px; border-radius: 8px; font-weight: bold; cursor: pointer; margin: 10px; }
        #click-target { width: 80px; height: 80px; background: #e94560; border-radius: 50%; position: absolute; cursor: pointer; }
    </style>
</head>
<body>
    <div id="menu">
        <h2>Choose Your Brain Break</h2>
        <button onclick="loadGame('clicker')">Space Junk Clicker</button>
        <button onclick="loadGame('reaction')">Focus Pulse</button>
        <button onclick="loadGame('random')">Surprise Me!</button>
    </div>

    <div id="game-box" class="hidden">
        <h3 id="game-title"></h3>
        <div id="game-content"></div>
        <button onclick="showMenu()" style="background:#555; color:white;">Back to Menu</button>
    </div>

    <script>
        const gameBox = document.getElementById('game-box');
        const menu = document.getElementById('menu');
        const content = document.getElementById('game-content');

        function loadGame(type) {
            menu.classList.add('hidden');
            gameBox.classList.remove('hidden');
            content.innerHTML = ""; // Clear old game
            
            if (type === 'random') {
                const types = ['clicker', 'reaction'];
                type = types[Math.floor(Math.random() * types.length)];
            }

            if (type === 'clicker') startClicker();
            if (type === 'reaction') startReaction();
        }

        // GAME 1: Space Junk Clicker
        function startClicker() {
            document.getElementById('game-title').innerText = "Whack the Space Junk!";
            let score = 0;
            content.innerHTML = `<p>Score: <span id='score'>0</span>/10</p><div id='area' style='height:200px; position:relative;'></div>`;
            const area = document.getElementById('area');
            
            function spawn() {
                const target = document.createElement('div');
                target.id = 'click-target';
                target.style.left = Math.random() * 80 + "%";
                target.style.top = Math.random() * 80 + "%";
                target.onclick = () => {
                    score++;
                    document.getElementById('score').innerText = score;
                    target.remove();
                    if (score < 10) spawn(); else win();
                };
                area.appendChild(target);
            }
            spawn();
        }

        // GAME 2: Focus Pulse (Reaction)
        function startReaction() {
            document.getElementById('game-title').innerText = "Wait for GREEN...";
            content.innerHTML = `<div id='pulse' style='width:100%; height:150px; background:red; border-radius:10px; cursor:pointer;'>CLICK WHEN GREEN</div>`;
            const pulse = document.getElementById('pulse');
            const delay = Math.random() * 3000 + 2000;
            
            setTimeout(() => {
                pulse.style.background = "#4ecca3";
                pulse.innerText = "CLICK NOW!";
                const startTime = Date.now();
                pulse.onclick = () => {
                    const time = Date.now() - startTime;
                    content.innerHTML = `<h3>Reaction: ${time}ms</h3>`;
                    win();
                };
            }, delay);
        }

        function win() {
            const winBtn = document.createElement('button');
            winBtn.innerText = "CLAIM ALIEN PRIZE 🎁";
            winBtn.onclick = () => alert("Logic to tell Glide you won!");
            content.appendChild(winBtn);
        }

        function showMenu() {
            menu.classList.remove('hidden');
            gameBox.classList.add('hidden');
        }
    </script>
</body>
</html>
