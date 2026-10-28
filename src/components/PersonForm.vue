<script setup lang="ts">
import { reactive } from 'vue'

interface PersonData {
  firstName: string
  lastName: string
  age: number | string
  weight: number | string
  city: string
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

const field: FieldConfig[] = [
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
</script>
