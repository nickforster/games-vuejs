<template>
  <div id="container">
    <div id="score">{{ score }}</div>
    <canvas id="game" width="600" height="400"></canvas>
  </div>
  <button id="back-button"><a href="#">back</a></button>
  <button id="reset-button" @click="resetGame">reset</button>
</template>
<script>
export default {
  data() {
    return {
      score: 0
    }
  },
  methods: {
    resetGame() {
      this.score = 0
    },
    updateSnake(e) {
      console.log(e.key)
    }
  },
  mounted() {
    this.resetGame()
    window.addEventListener('keydown', e => this.updateSnake(e));
  }
}
</script>
<style>
#container {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  height: 100vh;
}

#score {
  color: #492386;
  font-size: 3rem;
  user-select: none;
}

#game {
  border: 1px solid #492386;
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
