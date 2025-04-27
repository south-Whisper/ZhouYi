<template style="height: 100%;">
  <div class="entrance-container"  >
    <div :class="['box-container', { 'zoom-in': boxOpened }]" >
      <div class="box" @click="openBox">
  <div :class="['lid', { 'open-lid': boxOpened }]" ></div>
  <div class="lock" :class="{ 'lock-opened': boxOpened }"></div> <!-- 锁 -->
  <div class="center-block" ></div>
  <div class="base" ></div>
</div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'
const boxOpened = ref(false)
const emit = defineEmits(['boxOpened'])
function openBox() {
  boxOpened.value = true
  emit('boxOpened')
}

</script>

<style scoped>
.entrance-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: flex-start; /* 保证从顶部开始排列 */
  min-height: 100vh; /* 保证至少占满屏幕 */
  width: 100%;
  position: relative; /* 使 .base 相对于它定位 */
  padding-bottom: 50px; /* 给底部留出空间 */
}

.base {
  position: absolute;
  bottom: 0;
  width: 100%;
  background: #a9845b;
  border-radius: 0 0 4px 4px;
  z-index: 3;
  padding: 20px; /* 内边距，确保背景包裹内容 */
  min-height: 50%; /* 最小高度，避免背景消失 */
  height: auto; /* 根据内容自动调整高度 */
  box-sizing: border-box;
  margin-top:auto;
}
.box-container {
  transition: transform 1.5s ease;
}

.box-container.zoom-in {
  transform: scale(1.8) translateY(+80px) translateX(-400px);
}

.box {
  position: relative;
  width: 200px;
  height: 100px;
  cursor: pointer;
}

.lock {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 40px;
  height: 40px;
  background: #c0b7a5; /* 更像金属的颜色 */
  border-radius: 50%;
  border: 5px solid #5a4320; /* 锁外环，模拟金属边框 */
  box-shadow: 0 0 12px rgba(0, 0, 0, 0.5); /* 增加阴影让锁更立体 */
  z-index: 2;
  transition: transform 1s ease;
}

.lock-opened {
  transform: translate(-50%, -200%) rotate(180deg); /* 锁随盖子旋转并移动到上方 */
}

.lid {
  position: absolute;
  width: 100%;
  height: 50%;
  background: #8c6f4d;
  transform-origin: top center;
  transition: transform 1s ease;
  border-radius: 4px;
  z-index: 1;
}

.lid.open-lid {
  transform: rotateX(-120deg);
}

.center-block {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 60%;
  height: 10px;
  background: #fff;
  z-index: 0;
}


</style>
