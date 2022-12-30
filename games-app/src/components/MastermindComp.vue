<template>
  <div id="game">
    <div id="fields">
      <div class="field-container" v-for="(subFields, index) in fields" :key="index">
        <div class="field" v-for="(field, jindex) in subFields" :key="jindex">
          <span class="dot" :style="{ backgroundColor : field}" @click="unColor(index, jindex)"></span>
        </div>
      </div>
    </div>
    <div id="evaluations">
      <div class="evaluation-container" v-for="(subEvaluations, index) in evaluations" :key="index">
        <div class="evaluation" v-for="(evaluation, jindex) in subEvaluations" :key="jindex">
          {{ evaluation }}
        </div>
      </div>
    </div>
  </div>
  <button id="confirm" @click="confirmed">confirm</button>
  <div id="code" v-if="gameOver">
    <div v-for="(c, index) in code" :key="index"><span class="dot" :style="{ backgroundColor : c}"></span></div>
  </div>
  <div id="colors">
    <div v-for="(color, index) in colors" :key="index">
      <span class="dot" :style="{ backgroundColor : color}"
            @click="clickedColor(index)"></span>
    </div>
  </div>
  <button id="back-button"><a href="#">back</a></button>
  <button id="reset-button" @click="resetGame">reset</button>
</template>
<script>
export default {
  data() {
    return {
      fields: [],
      evaluations: [],
      code: [],
      colors: ["white", "black", "green", "red", "blue", "yellow", "orange"],
      selected: [],
      currentRow: 0,
      gameOver: false
    }
  },
  methods: {
    resetGame() {
      let array = []
      let array2 = []
      for (let i = 0; i < 10; i++) {
        for (let j = 0; j < 4; j++) {
          array[j] = "#492386"
          array2[j] = "⭘"
        }
        this.fields[i] = array
        this.evaluations[i] = [].slice.call(array2)
        array = []
      }

      for (let i = 0; i < 4; i++) {
        this.code[i] = this.colors[Math.floor(Math.random() * 7)]
      }
      this.selected = []
      this.currentRow = 0
    },
    clickedColor(i) {
      if (this.selected.length < 4) {
        this.selected.push(this.colors[i])
      }

      for (let j = 0; j < 4; j++) {
        if (this.selected[j] === "") {
          this.selected[j] = this.colors[i]
          break
        }
      }

      for (let j = 0; j < this.selected.length; j++) {
        if (this.selected[j] !== "") {
          this.fields[this.currentRow][j] = this.selected[j]
        }
      }
    },
    unColor(i, j) {
      this.fields[i][j] = "#492386"
      this.selected[j] = ""
    },
    confirmed() {
      if (this.selected.length === 4 && !this.selected.includes("")) {
        let outputArray = []

        let newCode = [].slice.call(this.code)

        for (let i = 0; i < this.selected.length; i++) {
          if (this.selected[i] === newCode[i]) {
            outputArray.push("⚫")
            newCode[i] = ""
          }
        }

        for (let i = 0; i < this.selected.length; i++) {
          if (newCode.includes(this.selected[i])) {
            outputArray.push("⚪")
            newCode[i] = ""
          }
        }


        for (let i = 0; i < outputArray.length; i++) {
          this.evaluations[this.currentRow][i] = outputArray[i]
        }
        this.currentRow++
        this.selected = []

        // game over
        let gameOver = true
        for (let i = 0; i < this.evaluations.length; i++) {
          if (this.evaluations[this.currentRow - 1][i] !== "⚫") {
            gameOver = false
          }
        }
        if (this.currentRow === 10 || gameOver) {
          this.gameOver = true
        }
      }
    }
  },
  mounted() {
    this.resetGame()
  }
}
</script>
<style scoped>
#game {
  display: flex;
  flex-direction: row;
  gap: 2rem;
  margin-top: 4rem;
}

#fields {
  position: relative;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  align-items: flex-end;
  width: 50%;
}

.field-container {
  display: flex;
  flex-direction: row;
}

.field {
  color: #492386;
  font-size: 2rem;
}

#evaluations {
  padding: 1rem 0;
  position: relative;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  align-items: flex-start;
  width: 50%;
}

.evaluation-container {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
}

.evaluation {
  color: #492386;
}

#code {
  display: flex;
  flex-direction: row;
  justify-content: center;
  width: 100%;
  margin-top: .5rem;
}

#colors {
  position: absolute;
  right: 1rem;
  top: 50%;
  transform: translateY(-50%);
}

.dot {
  height: 25px;
  width: 25px;
  border-radius: 50%;
  display: inline-block;
  border: 2px solid #492386;
}

#back-button, #reset-button, #confirm {
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

#confirm {
  bottom: 12%;
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
