<script setup lang="ts">
import { VueNipple } from '@/components'
import { ref } from 'vue'

const color = ref('blue')
function onMove(event: unknown, data: unknown) {
  console.log('Joystick moved:', event, data)
}
</script>

<template>
  <div class="container">
    <input v-model="color" type="text" />
    <VueNipple
      :options="{ color, mode: 'static', position: { top: '300px', left: '400px' } }"
      :style="{ height: '100%' }"
      @move="onMove"
    />
  </div>
</template>

<style scoped>
.container {
  width: 100%;
  height: 100vh;
  position: relative;
}
</style>
