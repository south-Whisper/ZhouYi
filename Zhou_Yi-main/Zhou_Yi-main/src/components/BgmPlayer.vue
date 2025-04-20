<template>
  <audio ref="audioRef" :src="audioSrc" loop autoplay hidden></audio>
</template>

<script setup>
import { onMounted, ref } from 'vue'

const audioSrc = '/bgm.flac' // 如果放在 public 目录，直接这样写就行
const audioRef = ref(null)

onMounted(() => {
  // 为了防止自动播放被拦截，可以尝试播放失败后引导用户点击
  const playAudio = () => {
    audioRef.value.play().catch(() => {
      console.warn('自动播放被拦截，用户操作后才能播放')
    })
  }

  document.addEventListener('click', playAudio, { once: true })
})
</script>
