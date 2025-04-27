<template>
  <div class="input-panel">
    <label>请输入您的出生日期：</label>
    <input type="date" v-model="birthDate" />
    <label>小时：</label>
    <input type="number" v-model="hour" min="0" max="23" />
    <br /><br />
    <button @click="onCalculate">算卦</button>
  </div>
</template>

<script setup>
import { ref } from 'vue';


const birthDate = ref('');
const hour = ref('');

const emit = defineEmits(['calculate']);

function onCalculate() {
  if (!birthDate.value) {
    alert('请输入出生日期');
    return;
  }
  if (!hour.value) {
    alert('请输入出生时间是几点');
    return;
  }
  emit('calculate', { birthDate: birthDate.value, hour: hour.value });
}
</script>
<style scoped>

.input-panel {
  position: absolute;
  top: 150px; /* 距离页面顶端的距离，根据匣子位置微调 */
  left: 50%;
  width: 800px;
  transform: translateX(-50%);
  background: white;
  padding: 20px;
  border-radius: 12px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
  opacity: 0;
  animation: fadeIn 2s ease forwards;
  animation-delay: 1s;
  z-index: 3; /* 确保在 box 后面 */
}


@keyframes fadeIn {
  to {
    opacity: 1;
  }
}

button {
  width: 100%;
  padding: 10px;
  background-color: #4CAF50;
  color: white;
  border: none;
  border-radius: 5px;
  font-size: 16px;
  cursor: pointer;
}

button:hover {
  background-color: #45a049;
}

@keyframes slideDown {
  from {
    opacity: 0;
    transform: translateY(-10px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

</style>