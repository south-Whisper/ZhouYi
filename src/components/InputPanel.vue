<template>
  <div class="input-panel">

    <br>

    <div>
      <label for="birthDate">请输入您的出生日期：</label>
      <input type="date" v-model="birthDate" />
      <label for="hour">小时：</label>
      <input type="number" v-model="hour" min="0" max="23">
    </div>

    <br>
    <br>

    <button @click="lunarCal">算卦</button>

    <div class="result" v-if="result" v-html="result">
    </div>
  </div>
</template>

<script setup>
import { Solar } from 'lunar-javascript';
import { ref } from 'vue'
import { marked } from 'marked'


const birthDate = ref('')
const result = ref('')
const hour = ref('')

//阴历ab算法
async function lunarCal(){
  if(!birthDate.value){
    alert('请输入出生日期')
    return
  }

  if(!hour.value){
    alert('请输入出生时间是几点')
    return
  }

  const date = new Date(birthDate.value);
  const solar = Solar.fromYmd(
      date.getFullYear(),
      date.getMonth() + 1,
      date.getDate()
  );

  const lunar = solar.getLunar();
  const lunarYear = lunar.getYear();     // 农历年（数字）
  const lunarMonth = lunar.getMonth();   // 农历月（数字）
  const lunarDay = lunar.getDay();       // 农历日（数字）
  const hour2 = hour.value;          // 使用阳历的小时

  // 3. 计算 a 和 b
  const a = (lunarYear + lunarMonth + lunarDay) % 8;
  const b = (lunarYear + lunarMonth + lunarDay + hour2) % 8;

  const idx = (8 - a) % 8 + (8 - b) % 8 * 8;

  const gua = [
     2, 23,  8, 20, 16, 35, 45, 12,
    15, 52, 39, 53, 62, 56, 31, 33,
     7,  4, 29, 59, 40, 64, 47,  6,
    46, 18, 48, 57, 32, 50, 28, 44,
    24, 27,  3, 42, 51, 21, 17, 25,
    36, 22, 63, 37, 55, 30, 49, 13,
    19, 41, 60, 61, 54, 38, 58, 10,
    11, 26,  5,  9, 34, 14, 43,  1
  ]

  const index = gua[idx];

  const guaContent = await getGuaMeaning(index);

  result.value = marked(`您的算卦结果是第${index}卦,\n\n${guaContent}`);

}

// 获取卦象含义
  async function getGuaMeaning(index) {
    if (index < 1 || index > 64) {
      throw new Error('卦序应在 1 到 64 之间');
    }

    try {
      const response = await fetch(`/64卦/${index}.md`);
      if (!response.ok) {
        throw new Error(`文件 ${index}.md 加载失败`);
      }

      const text = await response.text(); // 读取 Markdown 文件
      return text.trim(); // 返回干净的文本
    } catch (err) {
      console.error('获取卦象失败：', err);
      return '卦象加载失败';
    }
  }

</script>

<style scoped>

.input-panel {
  position: absolute;
  top: 325px; /* 距离页面顶端的距离，根据匣子位置微调 */
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

.result {
  margin-top: 20px;
  padding: 20px;
  background-color: #f9f9f9;
  border-radius: 10px;
  border: 1px solid #ddd;
  animation: slideDown 1s ease forwards;
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