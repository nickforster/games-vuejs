<template>
  <div id="container">
    <canvas id="game" width="450" height="450"></canvas>
  </div>
  <button id="back-button"><a href="#">back</a></button>
  <button id="reset-button" @click="resetGame">reset</button>
</template>
<script>
const canvas = {
  size: 450
}

const squareSize = canvas.size / 20
let pacman = {
  x: 9.5,
  y: 16
}
let ghosts = {
  blue: {
    x: 8,
    y: 10
  },
  green: {
    x: 9,
    y: 10
  },
  red: {
    x: 10,
    y: 10
  },
  yellow: {
    x: 11,
    y: 10
  }
}

const openMouth = new Image(2, 2)
openMouth.src = require(`@/assets/pacmanOpenMouth.png`)

const walking = new Image(2, 2)
walking.src = require(`@/assets/pacmanWalking.gif`)

const blueGhost = new Image(2, 2)
blueGhost.src = require(`@/assets/pacmanBlueGhost.png`)

const redGhost = new Image(2, 2)
redGhost.src = require(`@/assets/pacmanRedGhost.png`)

const greenGhost = new Image(2, 2)
greenGhost.src = require(`@/assets/pacmanGreenGhost.png`)

const yellowGhost = new Image(2, 2)
yellowGhost.src = require(`@/assets/pacmanYellowGhost.png`)

export default {
  data() {
    return {
      context: "",
      fields: [
        [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
        [0, 2, 2, 2, 2, 2, 2, 2, 2, 0, 0, 2, 2, 2, 2, 2, 2, 2, 2, 0],
        [0, 2, 0, 0, 2, 0, 0, 0, 2, 0, 0, 2, 0, 0, 0, 2, 0, 0, 2, 0],
        [0, 2, 0, 0, 2, 0, 0, 0, 2, 2, 2, 2, 0, 0, 0, 2, 0, 0, 2, 0],
        [0, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 0],
        [0, 2, 0, 0, 2, 0, 2, 0, 0, 0, 0, 0, 0, 2, 0, 2, 0, 0, 2, 0],
        [0, 2, 2, 2, 2, 0, 2, 2, 2, 0, 0, 2, 2, 2, 0, 2, 2, 2, 2, 0],
        [0, 2, 0, 0, 2, 0, 0, 0, 2, 0, 0, 2, 0, 0, 0, 2, 0, 0, 2, 0],
        [0, 2, 0, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 0, 2, 0],
        [0, 2, 2, 2, 2, 0, 2, 0, 3, 3, 3, 3, 0, 2, 0, 2, 2, 2, 2, 0],
        [0, 0, 0, 0, 2, 0, 2, 0, 1, 1, 1, 1, 0, 2, 0, 2, 0, 0, 0, 0],
        [1, 2, 2, 2, 2, 2, 2, 0, 0, 0, 0, 0, 0, 2, 2, 2, 2, 2, 2, 1],
        [0, 2, 0, 0, 2, 0, 2, 2, 2, 2, 2, 2, 2, 2, 0, 2, 0, 0, 2, 0],
        [0, 2, 2, 0, 2, 2, 2, 0, 0, 0, 0, 0, 0, 2, 2, 2, 0, 2, 2, 0],
        [0, 0, 2, 0, 2, 0, 2, 2, 0, 2, 2, 0, 2, 2, 0, 2, 0, 2, 2, 0],
        [0, 2, 2, 2, 2, 0, 0, 2, 0, 2, 2, 0, 2, 0, 0, 2, 2, 2, 2, 0],
        [0, 2, 0, 2, 2, 2, 0, 2, 2, 2, 2, 2, 2, 0, 2, 2, 2, 0, 2, 0],
        [0, 2, 0, 2, 0, 2, 0, 0, 0, 0, 0, 0, 0, 0, 2, 0, 2, 0, 2, 0],
        [0, 2, 2, 2, 0, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 0, 2, 2, 2, 0],
        [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0]
      ]
    }
  },
  methods: {
    resetGame() {
      this.context = document.getElementById("game").getContext("2d")
      this.drawContent()
    },
    drawContent() {
      for (let i = 0; i < this.fields.length; i++) {
        for (let j = 0; j < this.fields[i].length; j++) {
          switch (this.fields[i][j]) {
            case 0:
              this.context.fillStyle = "#492386";
              this.context.fillRect(j * squareSize, i * squareSize, squareSize, squareSize);
              break;
            case 1:
              this.context.fillStyle = "#1b0d31";
              this.context.fillRect(j * squareSize, i * squareSize, squareSize, squareSize);
              break;
            case 2:
              this.context.fillStyle = "#1b0d31";
              this.context.fillRect(j * squareSize, i * squareSize, squareSize, squareSize);
              this.context.fillStyle = "#ffee00";
              this.context.fillRect(j * squareSize + squareSize / 2 - 2.5, i * squareSize + squareSize / 2 - 2.5, 5, 5);
              break;
            case 3:
              this.context.fillStyle = "#1b0d31";
              this.context.fillRect(j * squareSize, i * squareSize, squareSize, squareSize);
              this.context.fillStyle = "#d200a6";
              this.context.fillRect(j * squareSize, i * squareSize, squareSize, 8);
              break;
          }
        }
      }
      this.context.drawImage(openMouth, pacman.x * squareSize, pacman.y * squareSize);

      this.context.drawImage(blueGhost, ghosts.blue.x * squareSize + 2, ghosts.blue.y * squareSize);
      this.context.drawImage(greenGhost, ghosts.green.x * squareSize + 2, ghosts.green.y * squareSize);
      this.context.drawImage(redGhost, ghosts.red.x * squareSize + 2, ghosts.red.y * squareSize);
      this.context.drawImage(yellowGhost, ghosts.yellow.x * squareSize + 2, ghosts.yellow.y * squareSize);
    }
  },
  mounted() {
    this.resetGame()
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
