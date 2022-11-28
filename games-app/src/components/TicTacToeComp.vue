<template>
  <div id="button-container">
    <button v-for="(field, index) in fields" :key="index" @click="updateFields(index)"
            :class="{ hasValue: fields[index] }" class="button">{{ field }}
    </button>
  </div>
  <button id="back-button"><a href="#">back</a></button>
  <button id="reset-button" @click="resetGame">reset</button>
  <span :class="{active: winner}" id="span"></span>
</template>

<script>
let player = 'x'

function checkWinner(fields) {
  for (let i = 0; i < fields.length; i += 3) {
    // check horizontal winner
    if (fields[i] === fields[i + 1] && fields[i] === fields[i + 2] && fields[i] !== '') {
      return {winner: fields[i], position: i, type: 'horizontal'}
    }
    // check vertical winner
    if (fields[i / 3] === fields[i / 3 + 3] && fields[i / 3] === fields[i / 3 + 6] && fields[i / 3] !== '') {
      return {winner: fields[i / 3], position: i / 3, type: 'vertical'}
    }
  }
  // check diagonal winners
  if (fields[0] === fields[4] && fields[0] === fields[8] && fields[0] !== '') {
    return {winner: fields[0], position: 0, type: 'diagonal-left-to-right'}
  }
  if (fields[2] === fields[4] && fields[2] === fields[6] && fields[2] !== '') {
    return {winner: fields[2], position: 2, type: 'diagonal-right-to-left'}
  }

  return {winner: '', position: -1}
}

export default {
  data() {
    return {
      fields: ['', '', '', '', '', '', '', '', ''],
      winner: '',
      spanHeight: 210,
      diagonalSpanHeight: 210 * Math.sqrt(2)
    }
  },
  methods: {
    updateFields(id) {
      if (this.fields[id] === '') {
        this.fields[id] = player;
        player === 'x' ? player = 'o' : player = 'x'

        // check and alert winner
        let returnValue = checkWinner(this.fields)
        this.winner = returnValue.winner
        if (this.winner !== '') {
          const rect = document.getElementsByClassName('button')[returnValue.position].getBoundingClientRect()
          let span = document.getElementById('span')
          if (returnValue.type === 'horizontal') {
            span.style.rotate = '90deg'
            span.style.top = rect.top + (rect.height - this.spanHeight) / 2 + 'px'
            span.style.left = rect.left + rect.width / 2 + this.spanHeight / 2 + 'px'
            span.style.height = this.spanHeight + 'px'
          } else if (returnValue.type === 'vertical') {
            span.style.rotate = '0deg'
            span.style.top = rect.top + rect.height / 2 + 'px'
            span.style.left = rect.left + rect.width / 2 + 'px'
            span.style.height = this.spanHeight + 'px'
          } else if (returnValue.type === 'diagonal-left-to-right') {
            span.style.rotate = '-45deg'
            span.style.top = rect.top + rect.height / 2 - (this.diagonalSpanHeight / 2 - this.diagonalSpanHeight / Math.sqrt(8)) + 'px'
            span.style.left = rect.left + rect.width / 2 + (this.diagonalSpanHeight / Math.sqrt(8)) + 'px'
            span.style.height = this.diagonalSpanHeight + 'px'
          } else {
            span.style.rotate = '45deg'
            span.style.top = rect.top + rect.height / 2 - (this.diagonalSpanHeight / 2 - this.diagonalSpanHeight / Math.sqrt(8)) + 'px'
            span.style.left = rect.left + rect.width / 2 - (this.diagonalSpanHeight / Math.sqrt(8)) + 'px'
            span.style.height = this.diagonalSpanHeight + 'px'
          }

          // make fields non-editable
          for (let i = 0; i < this.fields.length; i++) {
            this.fields[i] += ' '
          }
        }
      }
    },
    resetGame() {
      for (let i = 0; i < this.fields.length; i++) {
        this.fields[i] = ''
      }
      player = 'x'
      this.winner = ''
      const span = document.getElementById('span')
      span.style.height = '0px'
    }
  }
}
</script>

<style scoped>
body {
  background-color: #090a1a;
  margin: 0;
  scroll-behavior: smooth;
}

#button-container {
  display: grid;
  height: 100vh;
  grid-template-columns: repeat(3, 100px);
  justify-content: center;
  align-content: center;
}

button {
  height: 100px;
  aspect-ratio: 1;
  border: none;
  background-color: #090a1a;
  color: #492386;
  font-size: 80px;
  margin: 0;
  padding: 0;
  text-transform: uppercase;
  user-select: none;
}

button:nth-of-type(-n+6) {
  border-bottom: 1px solid #492386;
}

button:not(:nth-of-type(3n+3)) {
  border-right: 1px solid #492386;
}

button:not(:has(.hasValue)):hover {
  background-color: #2c1550 !important;
}

button.hasValue:hover {
  background-color: #090a1a !important;
}

#reset-button, #back-button {
  position: absolute;
  width: fit-content;
  height: fit-content;
  aspect-ratio: auto;
  bottom: 10%;
  left: 50%;
  transform: translateX(-50%);
  border: 1px solid #492386;
  border-radius: 6px;
  padding: .4rem;
  font-size: 1rem;
}

@media (max-height: 450px) {
  #reset-button {
    bottom: 0;
  }
}

#back-button {
  top: 1rem;
  left: 1rem;
  transform: none;
}

a {
  color: #492386;
  background: none;
  text-decoration: none;
}

a:hover, a:active, a:visited {
  color: #492386;
}

span {
  position: absolute;
  display: block;
  background-color: #492386;
  border-radius: 2px;
  width: 8px;
  margin: 0;
  padding: 0;
  height: 0;
  opacity: 0;
}

span.active {
  opacity: 1;
}
</style>
