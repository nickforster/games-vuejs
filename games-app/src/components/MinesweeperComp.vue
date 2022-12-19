<template>
  <div id="container">
    <button v-for="(field, index) in fields" v-bind:key="index" @click="buttonPressed(index)"
            :class="{hasValue: fields[index] !== '' && fields[index] !== '🏴'}"
            @contextmenu.prevent="buttonRightClick(index)">{{ field }}
    </button>
  </div>
  <button id="back-button"><a href="#">back</a></button>
  <button id="reset-button" @click="resetGame">reset</button>
  <div id="result-modal" v-if="gameOver !== ''">
    <h2>{{gameOver}}</h2>
  </div>
</template>
<script>
export default {
  data() {
    return {
      fields: [],
      solutionFields: [],
      firstGuess: true,
      gameOver: ""
    }
  },
  methods: {
    resetGame() {
      this.firstGuess = true
      this.fields = []
      this.solutionFields = []
      this.gameOver = ""
      for (let i = 0; i < 400; i++) {
        this.fields.push("")
        this.solutionFields.push(0)
      }

      // plant bombs
      for (let i = 0; i < 64; i++) {
        let randomIndex
        do {
          randomIndex = Math.floor(Math.random() * 399)
        } while (this.solutionFields[randomIndex] !== 0)
        this.solutionFields[randomIndex] = -1
      }

      // calculate amount of bombs for each field

      // calculate for top left position (0)
      if (this.solutionFields[0] !== -1) {
        if (this.solutionFields[1] === -1) this.solutionFields[0]++
        if (this.solutionFields[20] === -1) this.solutionFields[0]++
        if (this.solutionFields[21] === -1) this.solutionFields[0]++
      }
      // calculate position for top right corner (19)
      if (this.solutionFields[19] !== -1) {
        if (this.solutionFields[18] === -1) this.solutionFields[19]++
        if (this.solutionFields[38] === -1) this.solutionFields[19]++
        if (this.solutionFields[39] === -1) this.solutionFields[19]++
      }
      for (let i = 1; i < 19; i++) { // top row without corners
        if (this.solutionFields[i] !== -1) {
          if (this.solutionFields[i - 1] === -1) this.solutionFields[i]++
          if (this.solutionFields[i + 1] === -1) this.solutionFields[i]++
          if (this.solutionFields[i + 19] === -1) this.solutionFields[i]++
          if (this.solutionFields[i + 20] === -1) this.solutionFields[i]++
          if (this.solutionFields[i + 21] === -1) this.solutionFields[i]++
        }
      }

      // calculate position for bottom left corner (370)
      if (this.solutionFields[370] !== -1) {
        if (this.solutionFields[350] === -1) this.solutionFields[370]++
        if (this.solutionFields[351] === -1) this.solutionFields[370]++
        if (this.solutionFields[371] === -1) this.solutionFields[370]++
      }
      // calculate for bottom right position (399)
      if (this.solutionFields[399] !== -1) {
        if (this.solutionFields[368] === -1) this.solutionFields[399]++
        if (this.solutionFields[369] === -1) this.solutionFields[399]++
        if (this.solutionFields[398] === -1) this.solutionFields[399]++
      }

      for (let i = 371; i < 398; i++) { // bottom row without corners
        if (this.solutionFields[i] !== -1) {
          if (this.solutionFields[i - 21] === -1) this.solutionFields[i]++
          if (this.solutionFields[i - 20] === -1) this.solutionFields[i]++
          if (this.solutionFields[i - 19] === -1) this.solutionFields[i]++
          if (this.solutionFields[i - 1] === -1) this.solutionFields[i]++
          if (this.solutionFields[i + 1] === -1) this.solutionFields[i]++
        }
      }

      for (let i = 20; i < this.solutionFields.length - 20; i++) {
        if (this.solutionFields[i] !== -1) {
          if (this.solutionFields[i - 20] === -1) this.solutionFields[i]++
          if (this.solutionFields[i + 20] === -1) this.solutionFields[i]++

          if (i + 1 % 20 === 0) { // if position is in right row
            if (this.solutionFields[i - 21] === -1) this.solutionFields[i]++
            if (this.solutionFields[i - 1] === -1) this.solutionFields[i]++
            if (this.solutionFields[i + 19] === -1) this.solutionFields[i]++
          } else if (i % 20 === 0) { // if position is in left row
            if (this.solutionFields[i - 19] === -1) this.solutionFields[i]++
            if (this.solutionFields[i + 1] === -1) this.solutionFields[i]++
            if (this.solutionFields[i + 21] === -1) this.solutionFields[i]++
          } else { // if position is somewhere in the field
            if (this.solutionFields[i - 21] === -1) this.solutionFields[i]++
            if (this.solutionFields[i - 19] === -1) this.solutionFields[i]++
            if (this.solutionFields[i - 1] === -1) this.solutionFields[i]++
            if (this.solutionFields[i + 1] === -1) this.solutionFields[i]++
            if (this.solutionFields[i + 19] === -1) this.solutionFields[i]++
            if (this.solutionFields[i + 21] === -1) this.solutionFields[i]++
          }
        }
      }
    },
    buttonPressed(id) {
      if (this.firstGuess) {
        // make first thing open
        this.fields[id - 20] = this.solutionFields[id - 20]
        this.fields[id + 20] = this.solutionFields[id + 20]
        this.fields[id - 21] = this.solutionFields[id - 21]
        this.fields[id - 19] = this.solutionFields[id - 19]
        this.fields[id - 1] = this.solutionFields[id - 1]
        this.fields[id] = this.solutionFields[id]
        this.fields[id + 1] = this.solutionFields[id + 1]
        this.fields[id + 19] = this.solutionFields[id + 19]
        this.fields[id + 21] = this.solutionFields[id + 21]

        let array = [63, 62, 61, 60, 59, 58, 57, 43, 42, 41, 40, 39, 38, 37, 23, 22, 18, 17, 3, 2]
        for (let i = 0; i < array.length; i++) {
          if (Math.random() < 0.7) {
            this.fields[id - array[i]] = this.solutionFields[id - array[i]]
          }
          if (Math.random() < 0.7) {
            this.fields[id + array[i]] = this.solutionFields[id + array[i]]
          }
        }
        for (let i = 0; i < this.fields.length; i++) {
          if (this.fields[i] === -1) {
            this.fields[i] = '🏴'
          }
        }

        this.firstGuess = false
      } else {
        if (this.solutionFields[id] === -1) {
          this.gameOver = "Game over"
        } else {
          this.fields[id] = this.solutionFields[id]
        }
        this.gameOver = "You won"
        for (let i = 0; i < this.fields.length; i++) {
          if (this.fields[i] === '') {
            this.gameOver = ''
            break
          }
        }
      }
    },
    buttonRightClick(id) {
      this.fields[id] === '🏴' ? this.fields[id] = '' : this.fields[id] = '🏴'
    }
  },
  mounted() {
    this.resetGame()
  }
}
</script>
<style scoped>
#container {
  display: grid;
  height: 100vh;
  grid-template-columns: repeat(20, 25px);
  justify-content: center;
  align-content: center;
}

#container button {
  height: 100%;
  aspect-ratio: 1;
  background-color: #090a1a;
  color: #492386;
  margin: 0;
  padding: 0;
  user-select: none;
  border: 1px solid #492386;
}

button:hover {
  background-color: #2c1550 !important;
}

button.hasValue {
  background-color: #2c1550 !important;
}

#back-button, #reset-button {
  background-color: #090a1a;
  z-index: 500;
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
  bottom: 2%;
  left: 50%;
  transform: translateX(-50%);
  top: auto;
}

@media (max-height: 450px) {
  #reset-button {
    bottom: 0;
  }
}
</style>
