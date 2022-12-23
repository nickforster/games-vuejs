<template>
  <div id="game">
    <div v-for="(field, index) in fields" :key="index">
      <div class="field">{{ field }}</div>
    </div>
  </div>
  <button id="back-button"><a href="#">back</a></button>
  <button id="reset-button" @click="resetGame">reset</button>
</template>
<script>
export default {
  data() {
    return {
      fields: []
    }
  },
  methods: {
    resetGame() {

    }
  }
}
</script>
<style>

</style>
