<template>
  <div class="entrance-container">
    <div class="background-html" v-html="backgroundHtml"></div>
    <Box :boxOpened="showEngine" @boxOpened="showEngine = true" />
    <FortuneEngine v-if="showEngine" />
  </div>
</template>

<script setup>
import { ref } from 'vue'
import Box from './components/Box.vue'
import FortuneEngine from './components/FortuneEngine.vue'

const showEngine = ref(false)

const backgroundHtml = `
<!DOCTYPE html>
<html>
  <head>
    <meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
    <title>CSS3风水罗盘旋转动画特效</title>

    <link href="css/style.css" rel="stylesheet" type="text/css" />
  </head>

  <body>
    <div class="compass-main">
      <div class="compassdiv">
        <div class="compass-c"></div>
<!--        <div class="compass w h">-->
<!--          <div class="compass-1 w h"></div>-->
<!--          <div class="compass-2 w h"></div>-->
<!--          <div class="compass-3 w h"></div>-->
<!--          <div class="compass-4 w h"></div>-->
<!--          <div class="compass-5 w h"></div>-->
<!--          <div class="compass-6 w h"></div>-->
<!--          <div class="compass-7 w h"></div>-->
<!--          <div class="compass-8 w h"></div>-->
<!--          <div class="compass-9 w h"></div>-->
<!--          <div class="compass-10 w h"></div>-->
<!--          <div class="compass-11 w h"></div>-->
<!--          <div class="compass-12 w h"></div>-->
<!--          <div class="compass-13 w h"></div>-->
<!--          <div class="compass-14 w h"></div>-->
<!--          <div class="compass-15 w h"></div>-->
<!--          <div class="compass-16 w h"></div>-->
<!--          <div class="compass-17 w h"></div>-->
<!--          <div class="compass-18 w h"></div>-->
<!--          <div class="compass-19 w h"></div>-->
<!--        </div>-->
<!--      </div>-->
<!--    </div>-->
<!--  </body>-->
<!--</html>-->

`
</script>

<style>
html, body {
  height: 100%;
  margin: 0;
  padding: 0;
}

.background-html {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: -1; /* 确保背景在底层 */
  transform: scale(1.3) translateY(70px); /* 放大背景并向下移动 50px */
  transform-origin: center; /* 设置放大的中心点为背景的中心 */
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
