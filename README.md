<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Snake Game</title>
<style>
  body {
    background: black;
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
  }
  canvas {
    background: #111;
  }
</style>
</head>
<body>

<canvas id="game" width="400" height="400"></canvas>

<script>
const canvas = document.getElementById("game");
const ctx = canvas.getContext("2d");

let box = 20;
let snake = [{x: 200, y: 200}];
let direction = "RIGHT";
let food = {
  x: Math.floor(Math.random()*20)*box,
  y: Math.floor(Math.random()*20)*box
};

document.addEventListener("keydown", changeDirection);

function changeDirection(event) {
  if(event.key === "ArrowLeft" && direction !== "RIGHT") direction = "LEFT";
  if(event.key === "ArrowUp" && direction !== "DOWN") direction = "UP";
  if(event.key === "ArrowRight" && direction !== "LEFT") direction = "RIGHT";
  if(event.key === "ArrowDown" && direction !== "UP") direction = "DOWN";
}

function draw() {
  ctx.fillStyle = "#111";
  ctx.fillRect(0, 0, 400, 400);

  for(let i=0; i<snake.length; i++) {
    ctx.fillStyle = i === 0 ? "lime" : "green";
    ctx.fillRect(snake[i].x, snake[i].y, box, box);
  }

  ctx.fillStyle = "red";
  ctx.fillRect(food.x, food.y, box, box);

  let snakeX = snake[0].x;
  let snakeY = snake[0].y;

  if(direction === "LEFT") snakeX -= box;
  if(direction === "UP") snakeY -= box;
  if(direction === "RIGHT") snakeX += box;
  if(direction === "DOWN") snakeY += box;

  if(snakeX === food.x && snakeY === food.y) {
    food = {
      x: Math.floor(Math.random()*20)*box,
      y: Math.floor(Math.random()*20)*box
    };
  } else {
    snake.pop();
  }

  let newHead = {x: snakeX, y: snakeY};

  if(
    snakeX < 0 || snakeX >= 400 ||
    snkeY < 0 || snakeY >= 400 ||
    collision(newHead, snake)
  ) {
    clearInterval(game);
    alert("Game Over");
  }

  snake.unshift(newHead);
}

function collision(head, array) {
  for(let i = 0; i < array.length; i++) {
    if(head.x === array[i].x && head.y === array[i].y) {
      return true;
    }
  }
  return false;
}

let game = setInterval(draw, 120);
</script>

</body>
</html>