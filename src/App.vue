<template>
  <div class="entrance-container">
    <BgmPlayer />
    <Box @boxOpened="showInputPanel = true" />
    <InputPanel v-if="showInputPanel" />
  </div>
</template>

<script setup>
import { ref } from 'vue'
import Box from './components/Box.vue'
import InputPanel from './components/InputPanel.vue'
import BgmPlayer from './components/BgmPlayer.vue'

const showInputPanel = ref(false)
</script>


<style scoped>
.entrance-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 100vh;
  background: radial-gradient(#eae6df, #ccc);
  overflow: hidden;
}
</style>
