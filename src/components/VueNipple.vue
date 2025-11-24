<script setup lang="ts">
import { onMounted, shallowRef, useTemplateRef } from 'vue'
import nipplejs from 'nipplejs'

const manager = shallowRef<nipplejs.JoystickManager | null>(null)
const nippleRef = useTemplateRef<HTMLDivElement>('nipple')

onMounted(() => {
  manager.value = nipplejs.create({
    zone: nippleRef.value!,
    color: 'blue',
  })
})
</script>

<template>
  <div ref="nipple" class="nipple"></div>
</template>

<style scoped>
.nipple {
  position: relative;
}
</style>
