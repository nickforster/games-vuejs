<template>
  <HomeComp />
  <component :is="currentView" />
</template>

<script>
import RacingComp from "@/components/RacingComp";
import Game2048Comp from "@/components/Game2048Comp";
import HomeComp from "@/components/HomeComp";
import TicTacToeComp from "@/components/TicTacToeComp"

const routes = {
  '/': HomeComp,
  '/racing': RacingComp,
  '/2048': Game2048Comp,
  '/tictactoe': TicTacToeComp,
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
  margin: 0 1.5rem 3rem;
  scroll-behavior: smooth;
}
</style>
