<template>
  <main class="min-h-screen flex flex-col items-center justify-center p-4 text-center">
    <h1 class="text-3xl font-bold mb-4">🎲 Life Decision Maker</h1>

    <InputForm v-if="stage === 'input'" @submit="startFlip" />

    <Coin v-if="stage === 'flipping'" @finished="showResult" :choice="userChoice" />

    <Result v-if="stage === 'result'" :choice="userChoice" :result="flipResult" :ai-response="aiResponse" @reset="reset" />
  </main>
</template>

<script setup>
import { ref } from 'vue'
import InputForm from './components/InputForm.vue'
import Coin from './components/Coin.vue'
import Result from './components/Result.vue'

const stage = ref('input') // input | flipping | result
const userChoice = ref('')
const flipResult = ref('')
const aiResponse = ref('')

function startFlip(choice) {
  userChoice.value = choice
  stage.value = 'flipping'
}

async function showResult(result) {
  flipResult.value = result
  stage.value = 'result'

  const res = await fetch('https://your-cloudflare-worker-url.com', {
    method: 'POST',
    body: JSON.stringify({
      choice: userChoice.value,
      result,
    }),
  })
  const data = await res.json()
  aiResponse.value = data.text
}

function reset() {
  stage.value = 'input'
  userChoice.value = ''
  flipResult.value = ''
  aiResponse.value = ''
}
</script>
