index.html 
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Cross Zero Game</title>
    <style>
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }

        body {
            background: linear-gradient(135deg, #1e1e2f, #2d2b55);
            color: #fff;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            min-height: 100vh;
        }

        h1 {
            margin-bottom: 10px;
            font-size: 2.5rem;
            color: #00d2ff;
            text-shadow: 0 0 10px rgba(0, 210, 255, 0.5);
        }

        .status {
            font-size: 1.3rem;
            margin-bottom: 20px;
            font-weight: 600;
        }

        .board {
            display: grid;
            grid-template-columns: repeat(3, 90px);
            grid-template-rows: repeat(3, 90px);
            gap: 10px;
            background-color: rgba(255, 255, 255, 0.1);
            padding: 15px;
            border-radius: 15px;
            box-shadow: 0 8px 32px 0 rgba(0, 0, 0, 0.37);
        }

        .cell {
            background-color: #2a2a40;
            border-radius: 10px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 2.5rem;
            font-weight: bold;
            cursor: pointer;
            transition: background 0.3s, transform 0.1s;
            user-select: none;
        }

        .cell:active {
            transform: scale(0.95);
        }

        .cell.x {
            color: #ff4757;
        }

        .cell.o {
            color: #2ed573;
        }

        .reset-btn {
            margin-top: 25px;
            padding: 12px 25px;
            font-size: 1rem;
            font-weight: bold;
            color: #fff;
            background: linear-gradient(45deg, #ff4757, #ff6b81);
            border: none;
            border-radius: 25px;
            cursor: pointer;
            box-shadow: 0 4px 15px rgba(255, 71, 87, 0.4);
            transition: transform 0.2s;
        }

        .reset-btn:active {
            transform: scale(0.95);
        }
    </style>
</head>
<body>

    <h1>Cross Zero</h1>
    <div class="status" id="status">Player X ki Turn</div>

    <div class="board" id="board">
        <div class="cell" data-index="0"></div>
        <div class="cell" data-index="1"></div>
        <div class="cell" data-index="2"></div>
        <div class="cell" data-index="3"></div>
        <div class="cell" data-index="4"></div>
        <div class="cell" data-index="5"></div>
        <div class="cell" data-index="6"></div>
        <div class="cell" data-index="7"></div>
        <div class="cell" data-index="8"></div>
    </div>

    <button class="reset-btn" onclick="resetGame()">Restart Game</button>

    <script>
        const board = document.getElementById('board');
        const cells = document.querySelectorAll('.cell');
        const statusText = document.getElementById('status');

        let currentPlayer = 'X';
        let gameState = ["", "", "", "", "", "", "", "", ""];
        let gameActive = true;

        const winningConditions = [
            [0, 1, 2], [3, 4, 5], [6, 7, 8], // Rows
            [0, 3, 6], [1, 4, 7], [2, 5, 8], // Columns
            [0, 4, 8], [2, 4, 6]             // Diagonals
        ];

        cells.forEach(cell => {
            cell.addEventListener('click', handleCellClick);
        });

        function handleCellClick(e) {
            const clickedCell = e.target;
            const clickedIndex = parseInt(clickedCell.getAttribute('data-index'));

            if (gameState[clickedIndex] !== "" || !gameActive) {
                return;
            }

            gameState[clickedIndex] = currentPlayer;
            clickedCell.textContent = currentPlayer;
            clickedCell.classList.add(currentPlayer.toLowerCase());

            checkResult();
        }

        function checkResult() {
            let roundWon = false;

            for (let i = 0; i < winningConditions.length; i++) {
                const [a, b, c] = winningConditions[i];
                if (gameState[a] && gameState[a] === gameState[b] && gameState[a] === gameState[c]) {
                    roundWon = true;
                    break;
                }
            }

            if (roundWon) {
                statusText.textContent = `🎉 Player ${currentPlayer} Jeet Gaya!`;
                gameActive = false;
                return;
            }

            if (!gameState.includes("")) {
                statusText.textContent = "🤝 Game Draw Ho Gaya!";
                gameActive = false;
                return;
            }

            currentPlayer = currentPlayer === 'X' ? 'O' : 'X';
            statusText.textContent = `Player ${currentPlayer} ki Turn`;
        }

        function resetGame() {
            currentPlayer = 'X';
            gameState = ["", "", "", "", "", "", "", "", ""];
            gameActive = true;
            statusText.textContent = "Player X ki Turn";

            cells.forEach(cell => {
                cell.textContent = "";
                cell.classList.remove('x', 'o');
            });
        }
    </script>
</body>
</h>