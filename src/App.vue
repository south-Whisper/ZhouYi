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


<style>
/* 全局样式，确保背景色覆盖整个页面 */
html, body {
  height: 100%;
  margin: 0;
  padding: 0;
  background: radial-gradient(#eae6df, #ccc); /* 设置背景色 */
  overflow: auto; /* 浏览器自带滚动条 */
}
</style>

<style scoped>

.entrance-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center; /* 内容垂直居中 */
  min-height: 100vh;       /* 至少占满整个视口 */
  width: 100%;
  position: relative;      /* 为子元素的绝对定位提供基准 */
}

</style>
