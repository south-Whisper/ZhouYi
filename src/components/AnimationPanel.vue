<template>
  <div class="ap-out">
    <div v-if="show" class="animation-panel">
      <div class="spinner"></div>
      <p>正在推演卦象中...</p>
    </div>
  </div>
</template>

<script setup>
defineProps({
  show: {
    type: Boolean,
    required: true
  }
});
</script>

<style scoped>
.ap-out {
  display: flex;
  align-items: center;  /* 垂直居中 */
  justify-content: center;  /* 水平居中 */
  position: absolute; /* 确保它不随着页面滚动而影响位置 */
  top: 50%; /* 让它在页面中垂直居中 */
  left: 50%; /* 水平居中 */
  transform: translate(-50%, -50%); /* 精确居中 */
  width: 100%; /* 让它占满整个宽度 */
  height: 100%; /* 让它占满整个高度 */
}

.animation-panel {
  padding: 30px;
  background-color: #eef;
  text-align: center;
  border-radius: 10px;
  box-shadow: 0 0 8px rgba(0, 0, 0, 0.1);
  font-size: 18px;
}

/* 转圈圈 */
.spinner {
  width: 40px;
  height: 40px;
  margin: 0 auto 15px auto;
  border: 5px solid #ccc;
  border-top: 5px solid #4CAF50;
  border-radius: 50%;
  animation: spin 1s linear infinite;
}

/* 动画效果 */
@keyframes spin {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}
</style>
