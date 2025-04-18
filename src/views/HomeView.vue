<template>
  <div class="home">
    <h1>欢迎来到周易起卦</h1>
    <button @click="goToDivination">开始算卦</button>
  </div>
</template>

<script setup>
import { useRouter } from 'vue-router'
const router = useRouter()

const goToDivination = () => {
  router.push('/zhouyi') // 跳转到算卦页面
}
</script>

<style scoped>
.home {
  text-align: center;
  margin-top: 100px;
}
button {
  padding: 10px 20px;
  font-size: 18px;
}
</style>
