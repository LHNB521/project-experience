<template>
  <div ref="observedElement" class="observed-element">
    <!-- 这里是要监控的元素内容 -->
    <p v-if="isVisible">我是小明</p>
    <p v-else>我是小红</p>
  </div>
</template>

<script setup lang="ts">
const observedElement = ref<HTMLElement | null>(null)
const isVisible = ref(false)
let observer: any = null

onMounted(() => {
  observer = new IntersectionObserver((entires) => {
    entires.forEach((entry) => {
      if (entry.isIntersecting) {
        isVisible.value = true
      } else {
        isVisible.value = false
      }
    })
  })
  if (observedElement.value) {
    observer.observe(observedElement.value)
  }
})
onUnmounted(() => {
  if (observer && observedElement.value) {
    observer.unobserve(observedElement.value)
  }
})
</script>
<style lang="scss" scoped>
.observed-element {
  height: 100px;
  margin-top: 200vh; /* 使元素初始不在可视范围内 */
}
</style>
