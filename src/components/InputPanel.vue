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
import { Solar } from 'lunar-javascript';
import { marked } from 'marked';

const birthDate = ref('');
const hour = ref('');

const emit = defineEmits(['calculate']);

async function onCalculate() {
  if (!birthDate.value) {
    alert('请输入出生日期');
    return;
  }
  if (!hour.value) {
    alert('请输入出生时间是几点');
    return;
  }

  async function getGuaMeaning(index) {
    const response = await fetch(`/64卦/${index}.md`);
    const text = await response.text();
    return text.trim();
  }

  function getAncientTimeNumber(hour) {
    if (hour >= 23 || hour < 1) {
        return 1;  // 子时
    } else if (hour >= 1 && hour < 3) {
        return 2;  // 丑时
    } else if (hour >= 3 && hour < 5) {
        return 3;  // 寅时
    } else if (hour >= 5 && hour < 7) {
        return 4;  // 卯时
    } else if (hour >= 7 && hour < 9) {
        return 5;  // 辰时
    } else if (hour >= 9 && hour < 11) {
        return 6;  // 巳时
    } else if (hour >= 11 && hour < 13) {
        return 7;  // 午时
    } else if (hour >= 13 && hour < 15) {
        return 8;  // 未时
    } else if (hour >= 15 && hour < 17) {
        return 9;  // 申时
    } else if (hour >= 17 && hour < 19) {
        return 10;  // 酉时
    } else if (hour >= 19 && hour < 21) {
        return 11;  // 戌时
    } else if (hour >= 21 && hour < 23) {
        return 12;  // 亥时
    }
}

  const date = new Date(birthDate.value);
  const solar = Solar.fromYmd(
    date.getFullYear(),
    date.getMonth() + 1,
    date.getDate()
  );
  const lunar = solar.getLunar();const lunarYear = lunar.getYear();const lunarMonth = lunar.getMonth();const lunarDay = lunar.getDay();
  const hour2 = getAncientTimeNumber(parseInt(hour.value));

  const a = (lunarYear + lunarMonth + lunarDay) % 8;const b = (lunarYear + lunarMonth + lunarDay + hour2) % 8;
  const idx = (8 - a) % 8 + (8 - b) % 8 * 8;

  const gua = [
    2, 23, 8, 20, 16, 35, 45, 12,
    15, 52, 39, 53, 62, 56, 31, 33,
    7, 4, 29, 59, 40, 64, 47, 6,
    46, 18, 48, 57, 32, 50, 28, 44,
    24, 27, 3, 42, 51, 21, 17, 25,
    36, 22, 63, 37, 55, 30, 49, 13,
    19, 41, 60, 61, 54, 38, 58, 10,
    11, 26, 5, 9, 34, 14, 43, 1
  ];

  const index = gua[idx];
  const guaContent = await getGuaMeaning(index);
  const result = marked(`您的算卦结果是第${index}卦,\n\n${guaContent}`);

  emit('calculate', result);
}
</script>
<style scoped>

.input-panel {
  position: absolute;
  top: 0px; /* 距离页面顶端的距离，根据匣子位置微调 */
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