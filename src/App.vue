<script setup>
import { ref, watch } from 'vue'

const searchTerm = ref('')

const findProducts = async term => {}

watch(searchTerm, newTerm => findProducts(newTerm))
</script>

<template>
  <div class="w-full h-full flex flex-col gap-5 justify-center items-center">
    <h1 class="text-4xl font-bold">Gift Search Bar</h1>
    <input type="text" class="p-2 border-2 border-gray-dark" v-model="searchTerm" placeholder="Start typing..." />
    <ul class="list-disc">
      <li>Display suggestions here</li>
    </ul>
  </div>
</template>
