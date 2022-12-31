<template>
  <div id="hangman">
    <img :src="require(`@/assets/${'hangman'+wrongGuesses+'.png'}`)" alt="hanging man">
  </div>
  <div id="word">
    <div v-for="(s, index) in secret" :key="index">
      {{ s }}
    </div>
  </div>
  <div id="keyboard">
    <div>
      <button v-for="(key, index) in row1" :key="index" @click="letterPressed(key, 1, index)"
              :class="{cross: key==='❌', tick: key==='✓'}">{{ key }}
      </button>
    </div>
    <div>
      <button v-for="(key, index) in row2" :key="index" @click="letterPressed(key, 2, index)"
              :class="{cross: key==='❌', tick: key==='✓'}">{{ key }}
      </button>
    </div>
    <div>
      <button v-for="(key, index) in row3" :key="index" @click="letterPressed(key, 3, index)"
              :class="{cross: key==='❌', tick: key==='✓'}">{{ key }}
      </button>
    </div>
  </div>
  <button id="back-button"><a href="#">back</a></button>
  <button id="reset-button" @click="resetGame">reset</button>
</template>
<script>
import axios from "axios";

export default {
  data() {
    return {
      word: "",
      secret: [],
      wrongGuesses: 0,
      row1: ["q", "w", "e", "r", "t", "z", "u", "i", "o", "p"],
      row2: ["a", "s", "d", "f", "g", "h", "j", "k", "l"],
      row3: ["y", "x", "c", "v", "b", "n", "m"]
    }
  },
  methods: {
    resetGame() {
      axios.get("https://random-word-api.herokuapp.com/word").then(response => {
        this.word = response.data[0]
        this.secret = []
        for (let i = 0; i < this.word.length; i++) {
          this.secret[i] = "_"
        }
      })
      this.wrongGuesses = 0
      this.row1 = ["q", "w", "e", "r", "t", "z", "u", "i", "o", "p"]
      this.row2 = ["a", "s", "d", "f", "g", "h", "j", "k", "l"]
      this.row3 = ["y", "x", "c", "v", "b", "n", "m"]
    },
    letterPressed(letter, row, index) {
      if (letter !== "✓" && letter !== "❌") {
        if (this.word.includes(letter)) {
          for (let i = 0; i < this.word.length; i++) {
            if (this.word.charAt(i) === letter) {
              this.secret[i] = letter
            }
          }
          switch (row) {
            case 1:
              this.row1[index] = "✓"
              break
            case 2:
              this.row2[index] = "✓"
              break
            case 3:
              this.row3[index] = "✓"
          }
        } else {
          this.wrongGuesses++
          switch (row) {
            case 1:
              this.row1[index] = "❌"
              break
            case 2:
              this.row2[index] = "❌"
              break
            case 3:
              this.row3[index] = "❌"
          }
        }
      }
      this.gameOver()
    },
    gameOver() {
      if (this.wrongGuesses === 12) {
        this.secret = this.word
      }
    }
  },
  mounted() {
    this.resetGame()
  }
}
</script>
<style scoped>
#hangman {
  position: absolute;
  top: 0;
  left: 0;
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
}

img {
  height: 300px;
  width: auto;
  user-select: none;
}

#word {
  display: flex;
  flex-direction: row;
  justify-content: center;
  align-items: center;
  color: #492386;
  letter-spacing: .75rem;
  font-size: 2rem;
  height: 100vh;
  user-select: none;
}

#keyboard {
  position: absolute;
  bottom: 12%;
  width: 100%;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

#keyboard div button {
  background-color: #090a1a;
  color: #492386;
  margin: .25rem;
  width: 2.5rem;
  height: 3.2rem;
  text-transform: uppercase;
  user-select: none;
  border: 1px solid #492386;
  border-radius: 6px;
  padding: .4rem;
  font-size: 1.5rem;
}

#keyboard div button.cross {
  font-size: 1.2rem;
}

#keyboard div button.tick {
  font-size: 1.15rem;
  color: lawngreen;
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
