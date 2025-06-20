<template>
  <div class="w-full max-w-md">
    <input
      v-model="choice"
      type="text"
      placeholder="Enter your life question..."
      class="w-full p-3 border rounded-lg mb-4"
    />
    <button
      :disabled="!choice"
      @click="handleSubmit"
      class="bg-blue-500 text-white px-4 py-2 rounded hover:bg-blue-600 disabled:opacity-50"
    >
      Flip the Coin
    </button>
  </div>
</template>

<script setup>
import { ref } from 'vue'
const choice = ref('')
const emit = defineEmits(['submit'])

function handleSubmit() {
  const trimmed = choice.value.trim()

  // Basic validation rules
  if (!trimmed) return alert('Please enter a decision to make.')
  if (trimmed.length > 100) return alert('Too long! Keep it under 100 characters.')
  if (/[^a-zA-Z0-9\s'".,?!\-]/.test(trimmed)) return alert('Only basic punctuation and letters allowed.')

  emit('submit', trimmed)
  choice.value = ''
}
</script>
