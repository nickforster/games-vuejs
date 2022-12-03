<template>
  <img src="../assets/background.jpg" alt="background-image">
  <div>
    <p id="p">{{ randomFact }}</p>
  </div>
  <button id="back-button"><a href="#">back</a></button>
  <button id="reset-button" @click="getRandomFact">next</button>
  <div class="spotlight"></div>
</template>

<script>

import axios from "axios";

function typeWriter(text, n) {
  if (n < (text.length)) {
    const paragraph = document.getElementById('p')
    paragraph.innerText = text.substring(0, n + 1)
    n++;
    setTimeout(() => {
      typeWriter(text, n)
    }, 50);
  }
}

window.addEventListener('mousemove', (e) => {
  const spotlight = document.querySelector('.spotlight');
  console.log(spotlight)
  console.log(`radial-gradient(circle at ${e.pageX / window.innerWidth * 100}% ${e.pageY / window.innerHeight * 100}%, 160px}`)
  spotlight.style.background =
      `radial-gradient(
      circle at ${e.pageX / window.innerWidth * 100}% ${e.pageY / window.innerHeight * 100}%,
      transparent,
      rgb(09, 10, 26) 200px)`;

})

export default {
  data() {
    return {
      randomFact: "",
      allFonts: [],
    }
  },
  methods: {
    getRandomFact() {
      this.randomFact = ""
      axios.get("https://uselessfacts.jsph.pl/random.json?language=en").then(response => {
        let responseData = response.data
        typeWriter(responseData.text, 0)
      })

      let randomPosition = Math.floor(Math.random() * this.allFonts.length)
      const fontName = this.allFonts[randomPosition];

      document.getElementById("font").href = `https://fonts.googleapis.com/css?family=${fontName}`

      const paragraph = document.getElementById("p")
      paragraph.style.fontFamily = fontName
    }
  },
  mounted() {
    axios.get("https://www.googleapis.com/webfonts/v1/webfonts?key=AIzaSyBwIX97bVWr3-6AIUvGkcNnmFgirefZ6Sw").then(response => {
      for (let i = 0; i < response.data.items.length; i++) {
        this.allFonts[i] = response.data.items[i].family
      }
    }).then(() => {
      let randomPosition = Math.floor(Math.random() * this.allFonts.length)

      const link = document.createElement('link')
      link.rel = "stylesheet"
      link.id = "font"
      link.href = `https://fonts.googleapis.com/css?family=${this.allFonts[randomPosition]}`
      document.head.appendChild(link)

      this.getRandomFact()
    })
  },
  head: {
    link: [
      {rel: 'preload', id: 'current-font', as: 'font'},
      {rel: 'preload', id: 'loaded-font', as: 'font'}
    ]
  }
}
</script>

<style scoped>
img {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  object-fit: cover;
}

div:has(p) {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  width: 100vw;
  text-align: center;
}

p {
  position: relative;
  color: #492386;
  margin: 0;
  padding: 0;
  font-size: 50px;
  width: 80%;
  z-index: 400;
}

.spotlight {
  position: absolute;
  z-index: 100;
  left: 0;
  top: 0;
  height: 100%;
  width: 100%;
  background: radial-gradient(
      circle at 45% 45%,
      transparent 0px,
      rgb(09, 10, 26) 0px
  );
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
