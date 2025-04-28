<template>
  <div class="result-container" v-if="result">
    <div class="result-content" v-html="currentPageContent"></div>

    <div class="pagination" v-if="pages.length > 1">
      <button
        @click="prevPage"
        :disabled="currentPage === 0"
        class="page-button"
      >
        上一页
      </button>
      <span class="page-info">第 {{ currentPage + 1 }} 页 / 共 {{ pages.length }} 页</span>
      <button
        @click="nextPage"
        :disabled="currentPage === pages.length - 1"
        class="page-button"
      >
        下一页
      </button>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, watch } from 'vue';

const props = defineProps({
  result: {
    type: String,
    required: false
  }
});

const currentPage = ref(0);
const pageSize = 800; // 每页大约800个字符

// 将结果按段落分割
const pages = computed(() => {
  if (!props.result) return [];

  // 先按换行符分割段落
  const paragraphs = props.result.split('\n').filter(p => p.trim().length > 0);
  const resultPages = [];
  let currentPageContent = '';

  paragraphs.forEach(para => {
    // 如果当前页内容加上新段落不超过页面大小，就添加到当前页
    if (currentPageContent.length + para.length <= pageSize) {
      currentPageContent += para + '\n\n';
    } else {
      // 否则保存当前页，开始新的一页
      if (currentPageContent) {
        resultPages.push(currentPageContent);
      }
      currentPageContent = para + '\n\n';
    }
  });

  // 添加最后一页
  if (currentPageContent) {
    resultPages.push(currentPageContent);
  }

  return resultPages;
});

const currentPageContent = computed(() => {
  return pages.value[currentPage.value] || '';
});

function nextPage() {
  if (currentPage.value < pages.value.length - 1) {
    currentPage.value++;
  }
}

function prevPage() {
  if (currentPage.value > 0) {
    currentPage.value--;
  }
}

// 当结果变化时重置到第一页
watch(() => props.result, () => {
  currentPage.value = 0;
});
</script>

<style scoped>
.result-container {
  position: fixed;
  top: 55%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 80%;
  max-width: 800px;
  max-height: 80vh;
  padding: 20px;
  background-color: #f9f9f9;
  border-radius: 10px;
  border: 1px solid #ddd;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
  overflow-y: auto;
  animation: slideDown 1s ease forwards;
}

.result-content {
  line-height: 1.6;
  font-size: 16px;
}

.result-content >>> p {
  margin-bottom: 1em;
}

.pagination {
  display: flex;
  justify-content: center;
  align-items: center;
  margin-top: 20px;
  gap: 15px;
}

.page-button {
  padding: 8px 16px;
  background-color: #f0f0f0;
  border: 1px solid #ddd;
  border-radius: 4px;
  cursor: pointer;
  transition: all 0.3s;
}

.page-button:hover:not(:disabled) {
  background-color: #e0e0e0;
}

.page-button:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}

.page-info {
  font-size: 14px;
  color: #666;
}

@keyframes slideDown {
  from {
    opacity: 0;
    transform: translate(-50%, -60%);
  }
  to {
    opacity: 1;
    transform: translate(-50%, -50%);
  }
}
</style>