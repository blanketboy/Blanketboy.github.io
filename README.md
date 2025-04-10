body {
  margin: 0;
  font-family: 'Arial', sans-serif;
  background: #eef;
  text-align: center;
}

header {
  background-color: #ff5722;
  color: white;
  padding: 20px;
  font-size: 1.2em;
}

#score {
  margin-top: 10px;
  font-weight: bold;
}

main {
  padding: 20px;
}

#game-board {
  display: grid;
  grid-template-columns: repeat(10, 40px);
  gap: 5px;
  justify-content: center;
  margin: 20px auto;
}

.block {
  width: 40px;
  height: 40px;
  border-radius: 5px;
  cursor: pointer;
  transition: transform 0.1s;
}

.block:hover {
  transform: scale(1.1);
}

footer {
  background: #ddd;
  padding: 15px;
  margin-top: 40px;
}
const board = document.getElementById("game-board");
const scoreDisplay = document.getElementById("score");
const colors = ["red", "blue", "green", "orange", "purple"];
let grid = [];
let score = 0;
const size = 10;

function createGrid() {
  board.innerHTML = "";
  grid = [];
  for (let y = 0; y < size; y++) {
    grid[y] = [];
    for (let x = 0; x < size; x++) {
      const color = colors[Math.floor(Math.random() * colors.length)];
      const block = document.createElement("div");
      block.className = "block";
      block.style.backgroundColor = color;
      block.dataset.x = x;
      block.dataset.y = y;
      block.addEventListener("click", () => blast(x, y, color));
      board.appendChild(block);
      grid[y][x] = block;
    }
  }
}

function blast(x, y, color, visited = {}) {
  const key = `${x},${y}`;
  if (
    x < 0 || y < 0 || x >= size || y >= size ||
    visited[key] || !grid[y][x] || grid[y][x].style.backgroundColor !== color
  ) return;

  visited[key] = true;

  // Recursively check neighbors
  blast(x + 1, y, color, visited);
  blast(x - 1, y, color, visited);
  blast(x, y + 1, color, visited);
  blast(x, y - 1, color, visited);

  const cluster = Object.keys(visited);

  if (cluster.length > 1) {
    for (const k of cluster) {
      const [cx, cy] = k.split(",").map(Number);
      if (grid[cy][cx]) {
        grid[cy][cx].remove();
        grid[cy][cx] = null;
      }
    }
    score += cluster.length;
    scoreDisplay.textContent = `Score: ${score}`;
    dropBlocks();
  }
}

function dropBlocks() {
  for (let x = 0; x < size; x++) {
    for (let y = size - 1; y >= 0; y--) {
      if (!grid[y][x]) {
        for (let k = y - 1; k >= 0; k--) {
          if (grid[k][x]) {
            grid[y][x] = grid[k][x];
            grid[k][x] = null;
            grid[y][x].dataset.y = y;
            board.appendChild(grid[y][x]);
            break;
          }
        }
      }
    }
  }
}

function resetGame() {
  score = 0;
  scoreDisplay.textContent = "Score: 0";
  createGrid();
}

createGrid();