<template>
  <div id="game">
    <div v-for="(field, index) in fields" :key="index"><div class="field">{{ field }}</div></div>
  </div>
  <button id="back-button"><a href="#">back</a></button>
  <button id="reset-button" @click="resetGame">reset</button>
</template>

<script>

function addRandomNumber(fields) {
  let positions = []
  for (let i = 0; i < fields.length; i++) {
    if (fields[i] === '') {
      positions.push(i)
    }
  }
  let position = positions[Math.round(Math.random() * (positions.length - 1))]

  let randomNumber = Math.random()
  let numberToAdd = 4
  if (randomNumber <= 0.7) {
    numberToAdd = 2
  }
  if (position !== undefined) {
    fields[position] = numberToAdd
    return fields
  } else {
    return false
  }
}

function colorFields(fields, colors) {
  let fieldElements = document.getElementsByClassName('field')
  for (let i = 0; i < fields.length; i++) {
    if (fields[i] === '') {
      fieldElements[i].style.backgroundColor = '#090a1a'
    } else {
      fieldElements[i].style.backgroundColor = colors[fields[i]]
    }
  }
}

export default {
  data() {
    return {
      fields: ['', '', '', '', '', '', '', '', '', '', '', '', '', '', '', ''],
      colors: {
        2: '#0D0C21',
        4: '#110D28',
        8: '#150F2E',
        16: '#191035',
        32: '#1D123C',
        64: '#211343',
        128: '#251549',
        256: '#291750',
        512: '#2D1857',
        1024: '#311A5E',
        2048: '#351B64'
      }
    }
  },
  methods: {
    resetGame() {
      for (let i = 0; i < this.fields.length; i++) {
        this.fields[i] = ''
      }
      this.fields = addRandomNumber(this.fields)
      this.fields = addRandomNumber(this.fields)
      colorFields(this.fields, this.colors)
    },
    updateField(e) {
      switch (e.keyCode) {
        case 87:
        case 38:
          console.log("up")
          break;
        case 65:
        case 37:
          console.log("left")
          break;
        case 68:
        case 39:
          console.log("right")
          break;
        case 83:
        case 40:
          console.log("down")
          break;
      }

    },
  },
  mounted() {
    window.addEventListener('keydown', this.updateField);
    this.resetGame()
  },
}
</script>

<style scoped>
#game {
  position: absolute;
  top: 0;
  left: 0;
  display: grid;
  grid-template-columns: repeat(4, 100px);
  height: 100vh;
  width: 100vw;
  justify-content: center;
  align-content: center;
}

#game div {
  width: 100px;
  aspect-ratio: 1;
  display: flex;
  justify-content: center;
  align-content: center;
  flex-wrap: wrap;
  user-select: none;
  color: #492386;
  font-size: 2.25rem;
  height: 100%;
}

#game div:has(div) {
  border-right: 1px solid #492386 !important;
  border-top: 1px solid #492386;
}

#game div:nth-of-type(4n-3) {
  border-left: 1px solid #492386;
  width: 99px;
  aspect-ratio: auto;
}

#game div:nth-of-type(n+13) {
  border-bottom: 1px solid #492386;
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
  bottom: 10%;
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
