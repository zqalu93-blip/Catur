<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Tik Tak Tu</title>
  <style>
    * {
      box-sizing: border-box;
      font-family: 'Poppins', sans-serif;
    }

    body {
      margin: 0;
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh;
      background: linear-gradient(135deg, #74ebd5, #ACB6E5);
    }

    .game-container {
      background: white;
      border-radius: 20px;
      box-shadow: 0 8px 20px rgba(0,0,0,0.2);
      padding: 20px;
      text-align: center;
      width: 90%;
      max-width: 400px;
    }

    h1 {
      margin: 10px 0 20px;
      color: #333;
    }

    .board {
      display: grid;
      grid-template-columns: repeat(3, 1fr);
      gap: 10px;
    }

    .cell {
      width: 100%;
      aspect-ratio: 1/1;
      background: #f0f0f0;
      border-radius: 15px;
      font-size: 2.5em;
      display: flex;
      justify-content: center;
      align-items: center;
      cursor: pointer;
      transition: 0.3s;
      user-select: none;
    }

    .cell:hover {
      background: #e3e3e3;
    }

    .cell.x {
      color: #ff4b2b;
    }

    .cell.o {
      color: #00b09b;
    }

    .status {
      margin-top: 20px;
      font-weight: bold;
      color: #444;
    }

    button {
      margin-top: 15px;
      padding: 10px 20px;
      font-size: 1em;
      border: none;
      background: linear-gradient(135deg, #43cea2, #185a9d);
      color: white;
      border-radius: 10px;
      cursor: pointer;
      transition: 0.3s;
    }

    button:hover {
      transform: scale(1.05);
    }

    @media (hover: none) {
      .cell:hover {
        background: #f0f0f0;
      }
    }
  </style>
</head>
<body>
  <div class="game-container">
    <h1>🎯 Tik Tak Tu</h1>
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
    <div class="status" id="status">Giliran: X</div>
    <button id="resetBtn">🔄 Main Lagi</button>
  </div>

  <script>
    const cells = document.querySelectorAll('.cell');
    const statusText = document.getElementById('status');
    const resetBtn = document.getElementById('resetBtn');
    const board = document.getElementById('board');

    let currentPlayer = 'X';
    let gameActive = true;
    let boardState = Array(9).fill('');

    const winConditions = [
      [0,1,2],
      [3,4,5],
      [6,7,8],
      [0,3,6],
      [1,4,7],
      [2,5,8],
      [0,4,8],
      [2,4,6]
    ];

    function handleMove(index) {
      if (!gameActive || boardState[index] !== '') return;

      boardState[index] = currentPlayer;
      cells[index].textContent = currentPlayer;
      cells[index].classList.add(currentPlayer.toLowerCase());

      checkWinner();
      currentPlayer = currentPlayer === 'X' ? 'O' : 'X';
      if (gameActive) statusText.textContent = `Giliran: ${currentPlayer}`;
    }

    function checkWinner() {
      for (const [a,b,c] of winConditions) {
        if (boardState[a] && boardState[a] === boardState[b] && boardState[a] === boardState[c]) {
          gameActive = false;
          statusText.textContent = `🎉 Pemenang: ${boardState[a]}!`;
          return;
        }
      }
      if (!boardState.includes('')) {
        gameActive = false;
        statusText.textContent = '😐 Seri!';
      }
    }

    function resetGame() {
      boardState.fill('');
      gameActive = true;
      currentPlayer = 'X';
      statusText.textContent = 'Giliran: X';
      cells.forEach(cell => {
        cell.textContent = '';
        cell.classList.remove('x', 'o');
      });
    }

    cells.forEach((cell, index) => {
      // Klik dan gesture sentuh
      cell.addEventListener('click', () => handleMove(index));
      cell.addEventListener('touchstart', e => {
        e.preventDefault();
        handleMove(index);
      });
    });

    resetBtn.addEventListener('click', resetGame);
  </script>
</body>
</html>
