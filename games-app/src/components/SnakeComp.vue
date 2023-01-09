<template>
  <div id="container">
    <div id="score">{{ score }}</div>
    <canvas id="game" width="450" height="450"></canvas>
  </div>
  <button id="back-button"><a href="#">back</a></button>
  <button id="reset-button" @click="resetGame">reset</button>
</template>
<script>
const canvas = {
  size: 450
}

const amountSquares = 15
const squareSize = canvas.size / amountSquares
export default {
  data() {
    return {
      score: 0,
      context: "",
      snake: [
        {x: 4, y: 7},
        {x: 3, y: 7}
      ],
      direction: "right",
      foodPosition: {x: 9, y: 7},
      gameOn: false
    }
  },
  methods: {
    resetGame() {
      this.context = document.getElementById("game").getContext("2d")
      this.score = 0
      this.snake = [
        {x: 3, y: 7},
        {x: 4, y: 7}
      ]
      this.foodPosition = {x: 9, y: 7}
      this.direction = "right"
      this.gameOn = false
      this.drawGame()
    },
    drawGame() {
      // canvas background
      this.context.fillStyle = "#090a1a"
      this.context.fillRect(0, 0, canvas.size, canvas.size)
      for (let i = 0; i < canvas.size; i += squareSize * 2) {
        for (let j = 0; j < canvas.size; j += squareSize) {
          this.context.fillStyle = "#1b0d31";
          if (j % (2 * squareSize) === 0) {
            this.context.fillRect(i + squareSize, j, squareSize, squareSize);
          } else {
            this.context.fillRect(i, j, squareSize, squareSize);
          }
        }
      }

      // snake
      this.context.fillStyle = "#492386";
      for (let i = 0; i < this.snake.length; i++) {
        this.context.fillRect(this.snake[i].x * squareSize, this.snake[i].y * squareSize, squareSize, squareSize);
      }

      // food
      this.context.fillStyle = "#091a8c"
      this.context.beginPath();
      this.context.arc(this.foodPosition.x * squareSize + squareSize / 2, this.foodPosition.y * squareSize + squareSize / 2, 11, 0, Math.PI * 2, false);
      this.context.closePath();
      this.context.fill();
    },
    updateGame() {
      if (!this.gameOn) return

      // move snake
      let snakeX = this.snake[0].x;
      let snakeY = this.snake[0].y;

      if (this.direction === "up") snakeY--;
      if (this.direction === "down") snakeY++;
      if (this.direction === "left") snakeX--;
      if (this.direction === "right") snakeX++;

      // check if snake is eating food
      if (snakeX !== this.foodPosition.x || snakeY !== this.foodPosition.y) {
        this.snake.pop()
      } else {
        this.score++
        let randomX, randomY, state = true
        while (state) {
          randomX = Math.floor(Math.random() * amountSquares)
          randomY = Math.floor(Math.random() * amountSquares)
          state = false

          for (let i = 0; i < this.snake.length; i++) {
            if (this.snake[i].x === randomX && this.snake[i].y === randomY) {
              state = true
            }
          }
        }
        this.foodPosition.x = randomX
        this.foodPosition.y = randomY
      }

      let newHead = {
        x: snakeX,
        y: snakeY
      }
      this.snake.unshift(newHead);


      // check for game over
      for (let i = 1; i < this.snake.length; i++) {
        if (this.snake[0].x === this.snake[i].x && this.snake[0].y === this.snake[i].y){
          this.gameOn = false
          alert("game over");
        }
      }
      if (this.snake[0].x >= 15 || this.snake[0].x <= -1 || this.snake[0].y >= 15 || this.snake[0].y <= -1) {
        this.gameOn = false
        alert("game over");
      }

      this.drawGame()
    },
    updateDirection(e) {
      switch (e.key) {
        case "w":
        case "ArrowUp":
          if (this.direction !== "down") {
            this.direction = "up"
          }
          this.gameOn = true
          break;
        case "s":
        case "ArrowDown":
          if (this.direction !== "up") {
            this.direction = "down"
          }
          this.gameOn = true
          break;
        case "a":
        case "ArrowLeft":
          if (this.direction !== "right") {
            this.direction = "left"
          }
          this.gameOn = true
          break;
        case "d":
        case "ArrowRight":
          if (this.direction !== "left") {
            this.direction = "right"
          }
          this.gameOn = true
      }
    }
  }
  ,
  mounted() {
    this.resetGame()
    window.addEventListener('keydown', e => this.updateDirection(e));
    setInterval(this.updateGame, 200)
  }
}
</script>
<style scoped>
#container {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  height: 90vh;
}

#score {
  color: #492386;
  font-size: 3rem;
  user-select: none;
}

#game {
  border: 1px solid #1b0d31;
}

#back-button, #reset-button {
  background-color: #090a1a;
  color: #492386;
  margin: 0;
  text-transform: uppercase;
  user-select: none;
  position: absolute;
  width: fit-content;
  height: fit-content;
  border: 1px solid #492386;
  border-radius: 6px;
  padding: .4rem;
  font-size: 1rem;
  top: 1rem;
  left: 1rem;
}

a {
  color: #492386;
  background: none;
  text-decoration: none;
}

a:hover, a:active, a:visited {
  color: #492386;
}

#reset-button {
  bottom: 5%;
  left: 50%;
  transform: translateX(-50%);
  top: auto;
}

@media (max-height: 450px) {
  #reset-button {
    bottom: 0;
  }
}

button:hover {
  background-color: #2c1550 !important;
}
</style>
