<script setup lang="ts">
interface Props {
  type?: 'text' | 'number'
  minLength?: number
  min?: number | null
  max?: number | null
}
const props = withDefaults(defineProps<Props>(), {
  type: 'text',
  minLength: 0,
  min: null,
  max: null,
})
const model = defineModel<string | number>({ default: '' })

function clear() {
  model.value = ''
}
</script>
<template>
  <label><slot>Fallback text</slot></label>
  <input v-model="model" type="{{ type }}" />
  <button @click="clear" type="button">Clear</button>
</template>
