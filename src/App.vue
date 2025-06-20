<template>
  <main class="min-h-screen flex flex-col items-center justify-center p-4 text-center">
    <h1 class="text-3xl font-bold mb-4">🎲 Life Decision Maker</h1>

    <InputForm v-if="stage === 'input'" @submit="startFlip" />

    <Coin v-if="stage === 'flipping'" @finished="showResult" :choice="userChoice" />

    <div v-if="isLoading" class="mt-6 text-gray-500 animate-pulse">
      🤔 Thinking...
    </div>

    <Result v-if="stage === 'result'" :choice="userChoice" :result="flipResult" :ai-response="aiResponse"
      @reset="reset" />
  </main>
</template>

<script setup>
import { ref } from 'vue'
import InputForm from './components/InputForm.vue'
import Coin from './components/Coin.vue'
import Result from './components/Result.vue'

const stage = ref('input') // input | flipping | result
const isLoading = ref(false);
const userChoice = ref('')
const flipResult = ref('')
const aiResponse = ref('')

function sanitizeInput(text) {
  return text
    .trim()                      // Remove leading/trailing spaces
    .slice(0, 200)               // Limit to 200 chars
    .replace(/</g, "&lt;")       // Escape < to avoid HTML injection
    .replace(/>/g, "&gt;")
    .replace(/\s+/g, ' ');       // Normalize multiple spaces
}

function startFlip(choice) {
  userChoice.value = sanitizeInput(choice);
  stage.value = 'flipping'
}

async function showResult(choice, result) {
  isLoading.value = true;
  try {
    const prompt = `
      You are a wise, funny, and supportive life decision assistant.
      A person is trying to decide: "${choice}".
      The coin landed on: ${result.toUpperCase()}.
      - If HEADS: support the decision positively, give encouragement.
      - If TAILS: advise against it, warn them gently, or offer a humorous alternative.
      
      Be supportive, but do not contradict the coin’s decision.
      Keep the response under 50 words. Don't make medical, legal, or financial claims.
      `;

    const response = await puter.ai.chat(prompt);

    flipResult.value = result;
    aiResponse.value = response.message.content; // or just `response.message`
    userChoice.value = choice;
  } catch (err) {
    console.error('AI error:', err);
    aiResponse.value = 'Oops! Something went wrong.';
  } finally {
    isLoading.value = false;
    stage.value = 'result';
  }
}

function reset() {
  stage.value = 'input'
  userChoice.value = ''
  flipResult.value = ''
  aiResponse.value = ''
}
</script>
