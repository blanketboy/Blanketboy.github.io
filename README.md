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