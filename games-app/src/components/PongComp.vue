<template>
  <div id="container">
    <div id="score">
      <p>{{ player1Score }}</p>
      <p>{{ player2Score }}</p>
    </div>
    <canvas id="game" width="600" height="400"></canvas>
  </div>
  <button id="back-button"><a href="#">back</a></button>
  <div id="button-container">
    <button id="reset-button" @click="resetGame">reset</button>
    <button id="start-button" @click="startGame">{{ startButton }}</button>
  </div>
</template>
<script>
const canvas = {
  height: 400,
  width: 600
}

const ball = {
  x: canvas.width / 2 + 5,
  y: canvas.height / 2,
  r: 10,
  speed: 1,
  velocityX: 5,
  velocityY: 5,
  color: "white"
}

const player1 = {
  x: 10,
  y: canvas.height / 2 - 50,
  width: 10,
  height: 100,
  color: "white"
}

const player2 = {
  x: canvas.width - 20,
  y: canvas.height / 2 - 50,
  width: 10,
  height: 100,
  color: "white"
}

function collision(ball, player) {
  player.top = player.y;
  player.bottom = player.y + player.height;
  player.left = player.x;
  player.right = player.x + player.width;

  ball.top = ball.y - ball.r;
  ball.bottom = ball.y + ball.r;
  ball.left = ball.x - ball.r;
  ball.right = ball.x + ball.r;

  return ball.right > player.left && ball.bottom > player.top && ball.left < player.right && ball.top < player.bottom;
}

export default {
  data() {
    return {
      canvas: "",
      context: "",
      keys: [],
      player1Score: 0,
      player2Score: 0,
      gameOn: false,
      startButton: "start"
    }
  },
  methods: {
    resetGame() {
      this.canvas = document.getElementById("game")
      this.context = this.canvas.getContext("2d")

      ball.x = canvas.width / 2 + 5
      ball.y = canvas.height / 2
      ball.speed = 1
      ball.velocityX = 5
      ball.velocityY = 5
      player1.y = canvas.height / 2 - 50
      player2.y = canvas.height / 2 - 50

      this.drawContent()

      this.player1Score = 0
      this.player2Score = 0
    },
    startGame() {
      if (this.gameOn) {
        this.gameOn = false
        this.startButton = "start"
      } else {
        this.gameOn = true
        this.startButton = "stop"
      }
    },
    drawContent() {
      // canvas background
      this.context.fillStyle = "#090a1a"
      this.context.fillRect(0, 0, canvas.width, canvas.height)

      // ball
      this.context.fillStyle = ball.color
      this.context.beginPath();
      this.context.arc(ball.x, ball.y, ball.r, 0, Math.PI * 2, false);
      this.context.closePath();
      this.context.fill();

      // player 1
      this.context.fillStyle = player1.color;
      this.context.fillRect(player1.x, player1.y, player1.width, player1.height);

      // player 2
      this.context.fillStyle = player2.color;
      this.context.fillRect(player2.x, player2.y, player2.width, player2.height);

      // middle line
      for (let i = 0; i < 400; i += 62) {
        this.context.fillStyle = "white";
        this.context.fillRect(canvas.width / 2, i, 8, 30);
      }
    },
    updateBall() {
      if (this.gameOn) {
        this.drawContent()
        ball.x += ball.velocityX
        ball.y += ball.velocityY

        if (ball.y + ball.r > canvas.height || ball.y - ball.r < 0) {
          ball.velocityY = -ball.velocityY;
        }

        let player = (ball.x < canvas.width / 2) ? player1 : player2;
        if (collision(ball, player)) {
          ball.velocityX = -ball.velocityX;
        }

        if (ball.x - ball.r > canvas.width) {
          this.player1Score++
          ball.x = canvas.width / 2 + 5
          ball.y = canvas.height / 2
          ball.speed = 1
          ball.velocityX = 5
          ball.velocityY = 5
        } else if (ball.x + ball.r < 0) {
          this.player2Score++
          ball.x = canvas.width / 2 + 5
          ball.y = canvas.height / 2
          ball.speed = 1
          ball.velocityX = 5
          ball.velocityY = 5
        }
      }
    },
    updatePlayer() {
      const step = 10
      for (const key of this.keys) {
        switch (key) {
            // player 1
          case "w":
            if (player1.y > 0) {
              player1.y -= step
            }
            break;
          case "s":
            if (player1.y < canvas.height - 100) {
              player1.y += step
            }
            break;
            // player 2
          case "ArrowUp":
            if (player2.y > 0) {
              player2.y -= step
            }
            break;
          case "ArrowDown":
            if (player2.y < canvas.height - 100) {
              player2.y += step
            }
            break;
        }
      }
      this.drawContent()
    }
  },
  mounted() {
    window.addEventListener('keydown', (e) => {
      if (!this.keys.includes(e.key)) {
        this.keys.push(e.key)
      }
    });
    window.addEventListener('keyup', (e) => {
      if (this.keys.includes(e.key)) {
        const index = this.keys.indexOf(e.key)
        this.keys.splice(index, 1)
      }
    })
    this.resetGame()
    setInterval(() => {
      this.updateBall()
      this.updatePlayer()
    }, 10)
  }
}
</script>
<style scoped>
#container {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  flex-direction: column;
}

#score {
  display: flex;
  flex-direction: row;
  justify-content: space-evenly;
  width: 100%;
  user-select: none;
}

#score p {
  font-size: 3rem;
  color: #492386;
  margin: 0;
  padding: 0;
}

canvas {
  border: 2px solid #492386;
}

#back-button, #reset-button, #start-button {
  background-color: #090a1a;
  color: #492386;
  margin: 0;
  text-transform: uppercase;
  user-select: none;
  width: fit-content;
  height: fit-content;
  border: 1px solid #492386;
  border-radius: 6px;
  padding: .4rem;
  font-size: 1rem;
}

#back-button {
  position: absolute;
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

#button-container {
  position: absolute;
  width: 100%;
  bottom: 5%;
  display: flex;
  flex-direction: row;
  justify-content: center;
  gap: 1rem;
}

@media (max-height: 450px) {
  #button-container {
    bottom: 0;
  }
}

button:hover {
  background-color: #2c1550 !important;
}
</style>
