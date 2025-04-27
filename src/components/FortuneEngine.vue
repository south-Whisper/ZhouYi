<template>
  <div class="fortune-engine">
    <InputPanel @calculate="handleCalculate" />

    <AnimationPanel :show="loading" />

    <ResultPanel v-if="showResult" :result="result" />
  </div>
</template>

<script setup>
import { ref } from 'vue';
import { Solar } from 'lunar-javascript';
import { marked } from 'marked';
import InputPanel from '@/components/InputPanel.vue';
import AnimationPanel from '@/components/AnimationPanel.vue';
import ResultPanel from '@/components/ResultPanel.vue';

const result = ref('');
const loading = ref(false);
const showResult = ref(false);

async function handleCalculate({ birthDate, hour }) {
  loading.value = true;
  result.value = '';
  showResult.value = false;

  const date = new Date(birthDate);
  const solar = Solar.fromYmd(
    date.getFullYear(),
    date.getMonth() + 1,
    date.getDate()
  );
  const lunar = solar.getLunar();
  const lunarYear = lunar.getYear();
  const lunarMonth = lunar.getMonth();
  const lunarDay = lunar.getDay();
  const hour2 = hour;

  const a = (lunarYear + lunarMonth + lunarDay) % 8;
  const b = (lunarYear + lunarMonth + lunarDay + Number(hour2)) % 8;
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

  result.value = marked(`您的算卦结果是第${index}卦,\n\n${guaContent}`);

  await new Promise(resolve => setTimeout(resolve, 2000));

  loading.value = false;
  showResult.value = true;
}

async function getGuaMeaning(index) {
  if (index < 1 || index > 64) {
    throw new Error('卦序应在 1 到 64 之间');
  }
  const response = await fetch(`/64卦/${index}.md`);
  if (!response.ok) {
    throw new Error(`文件 ${index}.md 加载失败`);
  }
  const text = await response.text();
  return text.trim();
}
</script>

<style scoped>
.fortune-engine {
  height: 100vh;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding: 20px;
  box-sizing: border-box;
}
</style>
