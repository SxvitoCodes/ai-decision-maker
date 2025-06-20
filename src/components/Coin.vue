<template>
    <div class="coin-container" @click="startFlip" role="button" tabindex="0">
        <div ref="coin" class="coin">
            <div class="side heads">Heads</div>
            <div class="side tails">Tails</div>
        </div>
        <p class="mt-4 text-sm text-gray-500">Click the coin to flip</p>
    </div>
</template>

<script setup>
import { ref } from 'vue'
import { animate } from 'animejs'

const props = defineProps({
  choice: {
    type: String,
    required: true,
  }
})

const coin = ref(null)
const emit = defineEmits(['finished'])

function startFlip() {
  if (coin.value.classList.contains('flipping')) return

  coin.value.classList.add('flipping')

  const isHeads = Math.random() < 0.5
  const rotations = 6
  const finalRotation = rotations * 360 + (isHeads ? 0 : 180)

  animate(coin.value, {
    rotateY: finalRotation,
    duration: 2500,
    easing: 'easeInOutQuart',
    complete: () => {
      coin.value.classList.remove('flipping')
      coin.value.style.transform = `rotateY(${isHeads ? 0 : 180}deg)`
      emit('finished', props.choice, isHeads ? 'heads' : 'tails')
    },
  })
}
</script>

<style scoped>
.coin-container {
    perspective: 1000px;
    cursor: pointer;
    user-select: none;
    width: 150px;
    margin: 0 auto;
}

.coin {
    width: 150px;
    height: 150px;
    position: relative;
    transform-style: preserve-3d;
    transition: transform 0.6s;
}

.side {
    position: absolute;
    width: 150px;
    height: 150px;
    border-radius: 50%;
    backface-visibility: hidden;
    display: flex;
    justify-content: center;
    align-items: center;
    font-weight: bold;
    font-size: 1.5rem;
    color: white;
    user-select: none;
}

.heads {
    background: linear-gradient(135deg, #4ade80, #16a34a);
    /* green */
}

.tails {
    background: linear-gradient(135deg, #f87171, #b91c1c);
    /* red */
    transform: rotateY(180deg);
}
</style>
