<template>
  <div class="max-w-md mx-auto p-4 bg-white rounded shadow text-center">
    <h2 class="text-2xl font-semibold mb-2">Your Question:</h2>
    <p class="mb-6 italic text-gray-700">"{{ choice }}"</p>

    <h3 class="text-xl font-semibold mb-2">
      The coin says: 
      <span :class="flipResultClass">{{ flipResultText }}</span>
    </h3>

    <p class="mb-6 text-gray-800 whitespace-pre-line" v-if="aiResponse">
      {{ aiResponse }}
    </p>

    <button
      @click="$emit('reset')"
      class="bg-blue-500 text-white px-4 py-2 rounded hover:bg-blue-600"
    >
      Try Another Decision
    </button>
  </div>
</template>

<script setup>
const props = defineProps({
  choice: String,
  result: String,
  aiResponse: String,
})

const flipResultText = props.result === 'heads' ? 'YES' : 'NO'
const flipResultClass =
  props.result === 'heads' ? 'text-green-600 font-bold' : 'text-red-600 font-bold'
</script>
