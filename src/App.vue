<script setup lang="ts">
import ValidatedInput from './components/ValidatedInput.vue'
</script>

<template>
  <ValidatedInput type="text" :min-length="5">Hi this test</ValidatedInput>
  <ValidatedInput type="number" :min="2" :max="100">This is number field</ValidatedInput>
</template>
