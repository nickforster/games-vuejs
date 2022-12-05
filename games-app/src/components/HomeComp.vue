<template>
  <h1>Games</h1>
  <div id="games-container">
    <GameComp v-for="game in games" :key="game.name" :msg="game" />
  </div>
  <button @click="scrollToTop">&#8593;</button>
</template>
<script>
import GameComp from "@/components/GameComp";

export default {
  name: 'App',
  components: {GameComp},
  data() {
    return {
      // all the games and their attributes to be generated
      games: [
        {
          name: "2048",
          URL: "#/2048",
          imageURL: "2048.png",
          imageAlt: "2048 game preview",
          description: "Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam"
        },
        {
          name: "racing",
          URL: "#/racing",
          imageURL: "racing.png",
          imageAlt: "A picture of nascar racing",
          description: "Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam"
        },
        {
          name: "tic-tac-toe",
          URL: "#/tictactoe",
          imageURL: "tictactoe.png",
          imageAlt: "Tic-Tac-Toe game palette",
          description: "Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam"
        },
        {
          name: "pacman",
          URL: "#/pacman",
          imageURL: "pacman.png",
          imageAlt: "Preview of pacman game",
          description: "Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam"
        },
        {
          name: "Flags guessing",
          URL: "#/flagsguessing",
          imageURL: "flags.png",
          imageAlt: "Flags of the world",
          description: "Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam"
        },
        {
          name: "mastermind",
          URL: "#/mastermind",
          imageURL: "mastermind.png",
          imageAlt: "Preview of mastermind game",
          description: "Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam"
        },
        {
          name: "peg solitaire",
          URL: "#/pegsolitaire",
          imageURL: "peg-solitaire.png",
          imageAlt: "Preview of peg solitaire game",
          description: "Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam"
        },
        {
          name: "solitaire",
          URL: "#/solitaire",
          imageURL: "solitaire.png",
          imageAlt: "Preview of solitaire game",
          description: "Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam"
        },
        {
          name: "minesweeper",
          URL: "#/minesweeper",
          imageURL: "minesweeper.png",
          imageAlt: "Preview of minesweeper game",
          description: "Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam"
        },
        {
          name: "snake",
          URL: "#/snake",
          imageURL: "snake.png",
          imageAlt: "Preview of snake game",
          description: "Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam"
        },
        {
          name: "pong",
          URL: "#/pong",
          imageURL: "pong.png",
          imageAlt: "Preview of pong game",
          description: "Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam"
        },
        {
          name: "hangman",
          URL: "#/hangman",
          imageURL: "hangman.png",
          imageAlt: "Preview of hangman game",
          description: "Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam"
        },
        {
          name: "random facts",
          URL: "#/randomfacts",
          imageURL: "randomfacts.png",
          imageAlt: "Lightbulb idea icon",
          description: "Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam"
        }
      ],
    }
  },
  methods: {
    scrollToTop() {
      console.log("click")
      window.scrollTo({top: 0, behavior: 'smooth'});
    }
  }
}
</script>
<style scoped>
h1 {
  color: #492386;
  font-size: 80px;
  padding: 0;
  margin: 0;
  width: 100%;
  text-align: center;
}
#games-container {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 1rem;
  padding: 1.5rem;
}
button {
  width: 100%;
  text-align: right;
  padding: 0 2rem 0 0;
  border: none;
  border-radius: 50px;
  background-color: #090a1a;
  color: #492386;
  font-size: 3rem;
  font-weight: bold;
}

button:hover {
  color: #2c1550;
}
</style>

