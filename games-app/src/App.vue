<template>
  <component :is="currentView"/>
</template>

<script>
import HomeComp from "@/components/HomeComp";
import TicTacToeComp from "@/components/TicTacToeComp"
import FlagGuessingComp from "@/components/FlagGuessingComp";
import Game2048Comp from "@/components/Game2048Comp";
import RandomFactsComp from "@/components/RandomFactsComp";
import MinesweeperComp from "@/components/MinesweeperComp";
import PegSolitaireComp from "@/components/PegSolitaireComp";
import MastermindComp from "@/components/MastermindComp";
import HangmanComp from "@/components/HangmanComp";
import PongComp from "@/components/PongComp";
import SnakeComp from "@/components/SnakeComp"

const routes = {
  '/': HomeComp,
  '/tictactoe': TicTacToeComp,
  '/flagsguessing': FlagGuessingComp,
  '/2048': Game2048Comp,
  '/randomfacts': RandomFactsComp,
  '/minesweeper': MinesweeperComp,
  '/pegsolitaire': PegSolitaireComp,
  '/mastermind': MastermindComp,
  '/hangman': HangmanComp,
  '/pong': PongComp,
  '/snake': SnakeComp
}

export default {
  name: 'App',
  data() {
    return {
      currentPath: window.location.hash
    }
  },
  computed: {
    currentView() {
      return routes[this.currentPath.slice(1) || '/']
    }
  },
  mounted() {
    window.addEventListener('hashchange', () => {
      this.currentPath = window.location.hash
    })
  }
}
</script>

<style>
body {
  background-color: #090a1a;
  margin: 0;
  scroll-behavior: smooth;
}
</style>
