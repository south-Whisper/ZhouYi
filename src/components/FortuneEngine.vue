<template>
  <div class="fortune-engine">
    <InputPanel @calculate="handleResult" />
    <AnimationPanel :show="loading" />
    <ResultPanel v-if="showResult" :result="result" />
  </div>
</template>

<script setup>
import { ref } from 'vue';
import InputPanel from '@/components/InputPanel.vue';
import AnimationPanel from '@/components/AnimationPanel.vue';
import ResultPanel from '@/components/ResultPanel.vue';

const result = ref('');
const loading = ref(false);
const showResult = ref(false);

async function handleResult(calculatedResult) {
  loading.value = true;
  result.value = calculatedResult;
  showResult.value = false;

  await new Promise(resolve => setTimeout(resolve, 2000));

  loading.value = false;
  showResult.value = true;
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
