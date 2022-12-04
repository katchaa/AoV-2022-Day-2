<template>
  <div class="w-full h-full flex justify-center items-center">
    <div class="w-1/3 flex flex-col gap-2">
      <p data-qa="setup" class="text-white bg-emerald-800 w-2/3 px-3 py-4 rounded-lg">{{ jokeSetup }}</p>
      <p data-qa="delivery" v-if="isDeliveryVisible" class="self-end text-white bg-red-700 w-2/3 px-3 py-4 rounded-lg">
        {{ jokeDelivery }}
      </p>
      <button
        role="button"
        name="Another"
        v-if="isDeliveryVisible"
        @click="generateJoke()"
        class="text-white bg-green py-2 mt-2 rounded-lg hover:bg-green/80 transition duration-200"
      >
        Another
      </button>
      <button
        role="button"
        name="Tell Me!"
        v-else
        @click="showDelivery()"
        class="text-white bg-green py-2 mt-2 rounded-lg hover:bg-green/80 transition duration-200"
      >
        Tell Me!
      </button>
    </div>
  </div>
</template>

<script setup>
import { onMounted, ref } from 'vue'

// Get joke from API
const jokeSetup = ref('')
const jokeDelivery = ref('')

const generateJoke = async () => {
  isDeliveryVisible.value = false
  try {
    const response = await fetch('https://v2.jokeapi.dev/joke/christmas').then(res => res.json())
    jokeSetup.value = response.setup
    jokeDelivery.value = response.delivery
  } catch (err) {
    console.error(err.message)
  }
}
onMounted(() => generateJoke())

// Show joke's delivery
const isDeliveryVisible = ref(false)
const showDelivery = () => {
  isDeliveryVisible.value = true
}
</script>
