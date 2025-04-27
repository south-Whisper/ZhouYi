<template>
  <div class="entrance-container">
    <Box :boxOpened="showEngine" @boxOpened="showEngine = true" />
    <FortuneEngine v-if="showEngine" />
  </div>
</template>

<script setup>
import { ref } from 'vue'
import Box from './components/Box.vue'
import FortuneEngine from './components/FortuneEngine.vue'

const showEngine = ref(false)
</script>

<style>
html, body {
  height: 100%;
  margin: 0;
  padding: 0;
  background: radial-gradient(#eae6df, #ccc);
}
</style>

<style scoped>
.entrance-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 100vh;
  width: 100%;
  position: relative;
  overflow: hidden;
}
</style>
