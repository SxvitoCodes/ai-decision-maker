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
      @click="$emit('submit', choice)"
      class="bg-blue-500 text-white px-4 py-2 rounded hover:bg-blue-600 disabled:opacity-50"
    >
      Flip the Coin
    </button>
  </div>
</template>

<script setup>
import { ref } from 'vue'
const choice = ref('')
</script>
