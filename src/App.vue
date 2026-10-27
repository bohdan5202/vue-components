<script setup lang="ts">
import ValidatedInput from './components/ValidatedInput.vue'
</script>

<template>
  <ValidatedInput
    type="text"
    :min-length="5"
    @value-change="(v) => console.log('value:', v)"
    @error-validation="(e) => console.log('error:', e)"
    >Hi this test</ValidatedInput
  >
  <ValidatedInput type="number" :min="2" :max="100">This is number field</ValidatedInput>
</template>
