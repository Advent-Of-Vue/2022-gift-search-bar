<script setup>
import { ref, watch } from 'vue'
import { debounce } from 'debounce'

const isLoading = ref(false)
const searchTerm = ref('')
const products = ref([])

const findProducts = debounce(async term => {
  try {
    if (term.length === 0) return (products.value = [])

    isLoading.value = true

    const apiResponseBody = await (
      await fetch(`https://dummyjson.com/products/search?q=${term}&limit=5`)
    ).json()

    products.value = apiResponseBody.products
  } catch (error) {
    console.error(error)
    alert('Something went wrong!')
  } finally {
    isLoading.value = false
  }
}, 300)

watch(searchTerm, newSearchTerm => findProducts(newSearchTerm))
</script>

<template>
  <div class="w-full h-full flex flex-col gap-5 justify-center items-center">
    <h1 class="text-4xl font-bold">Gift Search Bar</h1>
    <input
      type="text"
      class="p-2 border-2 border-gray-dark disabled:opacity-40"
      v-model="searchTerm"
      placeholder="Start typing..."
      :disabled="isLoading"
    />
    <p v-if="isLoading" class="text-lg text-center">Please wait...</p>
    <ul v-else-if="!isLoading && products.length > 0" class="list-disc">
      <li v-for="product in products" :key="product.id">
        {{ product.title }} - ${{ product.price }}
      </li>
    </ul>
  </div>
</template>
