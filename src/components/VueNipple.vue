<script setup lang="ts">
import { onMounted, shallowRef, toRefs, useTemplateRef } from 'vue'
import nipplejs from 'nipplejs'

const props = defineProps<{
  options?: Omit<nipplejs.JoystickManagerOptions, 'zone'>
}>()
const emit = defineEmits<{
  (e: 'start', event: nipplejs.EventData, data: nipplejs.JoystickOutputData): void
  (e: 'end', event: nipplejs.EventData, data: nipplejs.JoystickOutputData): void
  (e: 'move', event: nipplejs.EventData, data: nipplejs.JoystickOutputData): void
  (e: 'dir', event: nipplejs.EventData, data: nipplejs.JoystickOutputData): void
  (e: 'dir:up', event: nipplejs.EventData, data: nipplejs.JoystickOutputData): void
  (e: 'dir:down', event: nipplejs.EventData, data: nipplejs.JoystickOutputData): void
  (e: 'dir:right', event: nipplejs.EventData, data: nipplejs.JoystickOutputData): void
  (e: 'dir:left', event: nipplejs.EventData, data: nipplejs.JoystickOutputData): void
  (e: 'plain', event: nipplejs.EventData, data: nipplejs.JoystickOutputData): void
  (e: 'plain:up', event: nipplejs.EventData, data: nipplejs.JoystickOutputData): void
  (e: 'plain:down', event: nipplejs.EventData, data: nipplejs.JoystickOutputData): void
  (e: 'plain:right', event: nipplejs.EventData, data: nipplejs.JoystickOutputData): void
  (e: 'plain:left', event: nipplejs.EventData, data: nipplejs.JoystickOutputData): void
  (e: 'shown', event: nipplejs.EventData, data: nipplejs.JoystickOutputData): void
  (e: 'hidden', event: nipplejs.EventData, data: nipplejs.JoystickOutputData): void
  (e: 'destroyed', event: nipplejs.EventData, data: nipplejs.JoystickOutputData): void
  (e: 'pressure', event: nipplejs.EventData, data: nipplejs.JoystickOutputData): void
  (e: 'added', event: nipplejs.EventData, data: nipplejs.JoystickOutputData): void
  (e: 'removed', event: nipplejs.EventData, data: nipplejs.JoystickOutputData): void
}>()
const { options } = toRefs(props)
const manager = shallowRef<nipplejs.JoystickManager | null>(null)
const nippleRef = useTemplateRef<HTMLDivElement>('nipple')

onMounted(() => {
  createNipple()
})

function createNipple() {
  if (manager.value) {
    manager.value.destroy()
  }
  manager.value = nipplejs.create({
    zone: nippleRef.value!,
    ...options.value,
  })
  manager.value.on('start', (event, data) => emit('start', event, data))
  manager.value.on('end', (event, data) => emit('end', event, data))
  manager.value.on('move', (event, data) => emit('move', event, data))
  manager.value.on('dir', (event, data) => emit('dir', event, data))
  manager.value.on('dir:up', (event, data) => emit('dir:up', event, data))
  manager.value.on('dir:down', (event, data) => emit('dir:down', event, data))
  manager.value.on('dir:right', (event, data) => emit('dir:right', event, data))
  manager.value.on('dir:left', (event, data) => emit('dir:left', event, data))
  manager.value.on('plain', (event, data) => emit('plain', event, data))
  manager.value.on('plain:up', (event, data) => emit('plain:up', event, data))
  manager.value.on('plain:down', (event, data) => emit('plain:down', event, data))
  manager.value.on('plain:right', (event, data) => emit('plain:right', event, data))
  manager.value.on('plain:left', (event, data) => emit('plain:left', event, data))
  manager.value.on('shown', (event, data) => emit('shown', event, data))
  manager.value.on('hidden', (event, data) => emit('hidden', event, data))
  manager.value.on('destroyed', (event, data) => emit('destroyed', event, data))
  manager.value.on('pressure', (event, data) => emit('pressure', event, data))
  manager.value.on('added', (event, data) => emit('added', event, data))
  manager.value.on('removed', (event, data) => emit('removed', event, data))
}
</script>

<template>
  <div ref="nipple" class="nipple"></div>
</template>

<style scoped>
.nipple {
  position: relative;
}
</style>
