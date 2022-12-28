<template>
  <div id="game">
    <div class="container" v-for="(subFields, index) in fields" :key="index">
      <div class="field" v-for="(field, jindex) in subFields" :key="jindex" @click="circlePressed(index, jindex)"
           :class="{ selectedField : selected[0] === index && selected[1] === jindex}">
        {{ field }}
      </div>
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
      selected: []
    }
  },
  methods: {
    resetGame() {
      let array = []
      for (let i = 0; i < 7; i++) {
        for (let j = 0; j < 7; j++) {
          array[j] = "⬤"
        }
        this.fields[i] = array
        array = []
      }

      for (let i = 0; i < 2; i++) {
        for (let j = 0; j < 2; j++) {
          this.fields[i][j] = ""
        }
        for (let j = 5; j < 7; j++) {
          this.fields[i][j] = ""
        }
      }
      for (let i = 5; i < 7; i++) {
        for (let j = 0; j < 2; j++) {
          this.fields[i][j] = ""
        }
        for (let j = 5; j < 7; j++) {
          this.fields[i][j] = ""
        }
      }
      this.fields[3][3] = "⭘"
    },
    circlePressed(i, j) {
      if (this.fields[i][j] === '⬤') {
        this.selected = [i, j]
      } else if (this.selected !== []) {
        const selectedI = this.selected[0]
        const selectedJ = this.selected[1]

        if (selectedI === i) {
          if (Math.abs(selectedJ - j) === 2) {
            this.fields[selectedI][selectedJ] = "⭘"
            this.fields[i][(selectedJ + j) / 2] = "⭘"
            this.fields[i][j] = "⬤"
          }
        } else if (selectedJ === j) {
          if (Math.abs(selectedI - i) === 2) {
            this.fields[selectedI][selectedJ] = "⭘"
            this.fields[(selectedI + i) / 2][j] = "⭘"
            this.fields[i][j] = "⬤"
          }
        }
        this.selected = []
      }
    },
  },
  mounted() {
    this.resetGame()
  }
}
</script>
<style scoped>
#game {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  width: 100%;
  height: 100vh;
}

.container {
  display: flex;
  flex-direction: row;
}

.field {
  color: #492386;
  font-size: 3rem;
  margin: 0;
  padding-right: .9rem;
  width: 48px;
  height: 64px;
  user-select: none;
}

.field:hover {
  color: #2c1550;
}

.selectedField {
  color: #091a8c;
}

.selectedField:hover {
  color: #091a8c;
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
