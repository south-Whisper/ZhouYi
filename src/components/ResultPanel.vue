<template>
  <div class="result" v-if="result" v-html="result"></div>
</template>

<script setup>
defineProps({
  result: {
    type: String,
    required: false
  }
});
</script>

<style scoped>
.result {
  position: fixed;
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
