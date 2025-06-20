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

async function showResult(choice, result) {
  try {
    const prompt = `The user is deciding: "${choice}". The coin landed on ${result.toUpperCase()}. Respond with a witty, creative, and thoughtful answer to support this decision.`;

    const response = await puter.ai.chat(prompt);

    flipResult.value = result;
    aiResponse.value = response.message.content; // or just `response.message`
    userChoice.value = choice;
    stage.value = 'result';
  } catch (err) {
    console.error('AI error:', err);
  }
}

function reset() {
  stage.value = 'input'
  userChoice.value = ''
  flipResult.value = ''
  aiResponse.value = ''
}
</script>
