<script setup lang="ts">
import { onMounted, onUnmounted, shallowRef, toRefs, useTemplateRef, watch } from 'vue'
import nipplejs from 'nipplejs'

type Options = Omit<nipplejs.JoystickManagerOptions, 'zone'>

const props = defineProps<{ options?: Options }>()
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

watch(
  () => options.value,
  (value, prev) => {
    if (JSON.stringify(value) !== JSON.stringify(prev)) {
      if (manager.value) {
        manager.value.destroy()
      }
      manager.value = createManager(nippleRef.value!, value)
    }
  },
)

onMounted(() => {
  manager.value = createManager(nippleRef.value!, options.value)
})

onUnmounted(() => {
  if (manager.value) {
    manager.value.destroy()
    manager.value = null
  }
})

function createManager(zone: HTMLElement, options: Options = {}) {
  const manager = nipplejs.create({ zone, ...options })
  manager.on('start', (event, data) => emit('start', event, data))
  manager.on('end', (event, data) => emit('end', event, data))
  manager.on('move', (event, data) => emit('move', event, data))
  manager.on('dir', (event, data) => emit('dir', event, data))
  manager.on('dir:up', (event, data) => emit('dir:up', event, data))
  manager.on('dir:down', (event, data) => emit('dir:down', event, data))
  manager.on('dir:right', (event, data) => emit('dir:right', event, data))
  manager.on('dir:left', (event, data) => emit('dir:left', event, data))
  manager.on('plain', (event, data) => emit('plain', event, data))
  manager.on('plain:up', (event, data) => emit('plain:up', event, data))
  manager.on('plain:down', (event, data) => emit('plain:down', event, data))
  manager.on('plain:right', (event, data) => emit('plain:right', event, data))
  manager.on('plain:left', (event, data) => emit('plain:left', event, data))
  manager.on('shown', (event, data) => emit('shown', event, data))
  manager.on('hidden', (event, data) => emit('hidden', event, data))
  manager.on('destroyed', (event, data) => emit('destroyed', event, data))
  manager.on('pressure', (event, data) => emit('pressure', event, data))
  manager.on('added', (event, data) => emit('added', event, data))
  manager.on('removed', (event, data) => emit('removed', event, data))
  return manager
}
</script>

<template>
  <div ref="nipple"></div>
</template>
