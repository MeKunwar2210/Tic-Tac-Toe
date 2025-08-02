<!DOCTYPE html>
<html>
<head>
  <title>Tic Tac Toe</title>
  <style>
    body {
      font-family: 'Courier New', monospace;
      text-align: center;
      margin: 0;
      background-color: #f0f8ff; /* Light background */
      color: #000;
      overflow: hidden;
    }

    h1 {
      margin-top: 20px;
    }

    #board {
      display: grid;
      grid-template-columns: repeat(3, 100px);
      grid-gap: 5px;
      justify-content: center;
      margin: 20px auto;
    }

    .cell {
      width: 100px;
      height: 100px;
      font-size: 48px;
      background-color: #fff;
      border: 2px solid #999;
      display: flex;
      align-items: center;
      justify-content: center;
      cursor: pointer;
      user-select: none;
    }

    .cell.X { color: red; }
    .cell.O { color: blue; }

    #status {
      margin-top: 10px;
      font-size: 20px;
    }

    #reset {
      margin-top: 20px;
      padding: 10px 20px;
      font-size: 16px;
      cursor: pointer;
      background-color: #4CAF50;
      color: white;
      border: none;
      border-radius: 6px;
    }

    #win-screen {
      position: fixed;
      top: 0;
      left: 0;
      width: 100vw;
      height: 100vh;
      background-color: black;
      color: lime;
      display: none;
      align-items: center;
      justify-content: center;
      z-index: 999;
      font-family: 'Courier New', monospace;
      font-size: 80px;
      letter-spacing: 10px;
      animation: pixelFade 0.8s ease-in-out;
    }

    @keyframes pixelFade {
      from {
        transform: scale(0.5);
        opacity: 0;
        filter: blur(5px);
      }
      to {
        transform: scale(1);
        opacity: 1;
        filter: none;
      }
    }

    @media (max-width: 600px) {
      #win-screen {
        font-size: 50px;
      }
      .cell {
        width: 80px;
        height: 80px;
        font-size: 36px;
      }
    }
  </style>
</head>
<body>

  <div id="win-screen">YOU WIN!</div>

  <h1>Tic Tac Toe</h1>
  <div id="board"></div>
  <div id="status">Player <span style="color: red;">X</span>'s turn</div>
  <button id="reset" onclick="resetGame()">Reset Game</button>

  <script>
    const board = document.getElementById("board");
    const status = document.getElementById("status");
    const winScreen = document.getElementById("win-screen");

    let currentPlayer = "X";
    let cells = ["", "", "", "", "", "", "", "", ""];
    let gameActive = true;

    function showWinScreen(winner) {
      winScreen.style.display = "flex";
      winScreen.textContent = `PLAYER ${winner} WINS!`;
      setTimeout(() => winScreen.style.display = "none", 3000); // Hide after 3 seconds
    }

    function checkWinner() {
      const winPatterns = [
        [0,1,2], [3,4,5], [6,7,8],
        [0,3,6], [1,4,7], [2,5,8],
        [0,4,8], [2,4,6]
      ];
      for (let pattern of winPatterns) {
        const [a, b, c] = pattern;
        if (cells[a] && cells[a] === cells[b] && cells[a] === cells[c]) {
          gameActive = false;
          showWinScreen(cells[a]);
          status.innerHTML = `<span style="color: ${cells[a] === 'X' ? 'red' : 'blue'};">Player ${cells[a]} wins!</span>`;
          return;
        }
      }
      if (!cells.includes("")) {
        gameActive = false;
        status.innerHTML = `<span style="color: orange;">It's a draw!</span>`;
      }
    }

    function cellClick(i) {
      if (cells[i] === "" && gameActive) {
        cells[i] = currentPlayer;
        renderBoard();
        checkWinner();
        if (gameActive) {
          currentPlayer = currentPlayer === "X" ? "O" : "X";
          status.innerHTML = `Player <span style="color: ${currentPlayer === 'X' ? 'red' : 'blue'};">${currentPlayer}</span>'s turn`;
        }
      }
    }

    function renderBoard() {
      board.innerHTML = "";
      cells.forEach((cell, i) => {
        const div = document.createElement("div");
        div.className = "cell";
        if (cell) {
          div.classList.add(cell);
          div.textContent = cell;
        }
        div.onclick = () => cellClick(i);
        board.appendChild(div);
      });
    }

    function resetGame() {
      cells = ["", "", "", "", "", "", "", "", ""];
      currentPlayer = "X";
      gameActive = true;
      status.innerHTML = `Player <span style="color: red;">X</span>'s turn`;
      renderBoard();
    }

    renderBoard();
  </script>

</body>
</html>
