<script setup lang="ts">
import { ref, onMounted } from 'vue'

const video = ref<HTMLVideoElement | null>(null)
onMounted(() => {
  if (video.value) {
    video.value.play()
  }
})
</script>

<template>
  <main>
    <video ref="video" poster="@/assets/petticodestill-3.png" muted>
      <source src="@/assets/petticode-teaser.webm#t=0,1.4" type="video/webm" />
    </video>
  </main>
</template>

<style lang="scss">
video {
  width: 100vw;
  height: 100vh;
  object-fit: contain;
  position: fixed;
  top: 0;
  left: 0;
}
</style>
