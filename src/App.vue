<script setup>
import Title from './components/Title.vue'
import ScoreBoard from './components/ScoreBoard.vue'
</script>

<template>
  <div class="thegrid">
    <Title msg="Scoreboard" />
    <ScoreBoard />
  </div>
</template>

<style scoped>
.thegrid{
  height: 100vh;
  display: grid;
  grid-template-columns: 1fr;
  grid-template-rows: 200px auto;
}
</style>
