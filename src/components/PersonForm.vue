<script setup lang="ts">
import { reactive, ref, computed } from 'vue'
import ValidatedInput from './ValidatedInput.vue'

interface PersonData {
  firstName: string | number
  lastName: string | number
  age: string | number
  weight: string | number
  city: string | number
}

interface FieldConfig {
  key: keyof PersonData
  label: string
  type: 'text' | 'number'
  minLength?: number
  min?: number
  max?: number
}

const form = reactive<PersonData>({
  firstName: '',
  lastName: '',
  age: '',
  weight: '',
  city: '',
})

const fields: FieldConfig[] = [
  { key: 'firstName', label: 'First name', type: 'text', minLength: 2 },
  { key: 'lastName', label: 'Last name', type: 'text', minLength: 2 },
  { key: 'age', label: 'Age', type: 'number', min: 1, max: 105 },
  { key: 'weight', label: 'Weight', type: 'number', min: 30, max: 300 },
  { key: 'city', label: 'City', type: 'text', minLength: 3 },
]

const errors = reactive<Record<keyof PersonData, string | null>>({
  firstName: null,
  lastName: null,
  age: null,
  weight: null,
  city: null,
})
const sent = ref(false)
const appCount = ref(0)
const anyError = computed(() => Object.values(errors).some((e) => e !== null))

function send() {
  sent.value = true
}

function sentClear() {
  sent.value = false
}

function clearEveryThing() {
  sentClear()
  appCount.value++
  for (const f of fields) {
    form[f.key] = ''
  }
}
</script>
<template>
  <div class="person-form">
    <div v-if="sent" class="summary">
      <p v-for="f in fields" :key="`${f.key}-${appCount}`">{{ f.label }}: {{ form[f.key] }}</p>
      <button @click="sentClear">Hide send</button>
    </div>
    <div>
      <form @submit.prevent="send">
        <ValidatedInput
          v-for="field in fields"
          :key="`${field.key}-${appCount}`"
          :type="field.type"
          :min-length="field.minLength"
          :max="field.max"
          :min="field.min"
          v-model="form[field.key]"
          @error-validation="(e) => (errors[field.key] = e)"
          @value-change="(v) => console.log('value:', v)"
        >
          {{ field.label }}
        </ValidatedInput>
        <button type="submit" :disabled="anyError">Submit</button>
        <button type="button" @click="clearEveryThing">Clear All</button>
      </form>
    </div>
  </div>
</template>
