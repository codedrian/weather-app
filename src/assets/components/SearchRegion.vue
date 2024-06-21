<script setup>
import {ref, defineEmits} from 'vue'

const searchTerm = ref({
  query: '',
  timeout: null,
  response: null
})
const searchArr = ref([])

function handleSearch() {
  /*NOTE: This code clears the previous search  */
  clearTimeout(searchTerm.value.timeout)
  searchTerm.value.timeout = setTimeout(async () => {
    searchArr.value.push(searchTerm.value.query);
    if (searchTerm.value.query != '') {
      const response = await fetch(`http://api.weatherapi.com/v1/search.json?key=3a9fbba54bf048308a652424241306&q=${searchTerm.value.query}`)
      const data = await response.json();
      searchTerm.value.response = data;
    }
  }, 500);
}

const emit = defineEmits(['place-data'])

async function searchForecast(id) {
  const response = await fetch(`http://api.weatherapi.com/v1/forecast.json?key=3a9fbba54bf048308a652424241306&q=id:${id}&days=3&aqi=no&alerts=no`)
  const data = await response.json();
  emit('place-data', data);
  searchTerm.value.query = "";
  searchTerm.value.response = null;
}

const date = new Date().toLocaleDateString('en-us', {
  weekday: 'long',
  year: 'numeric',
  month: 'long',
  day: 'numeric'
})
</script>

<template>
  <p class="mx-auto max-w-md text-center text-2xl font-bold my-11 font-mono">{{ date }}</p>

  <form class="max-w-md mx-auto">
    <label for="default-search" class="mb-2 text-sm font-medium text-gray-900 sr-only dark:text-white">Search</label>
    <div class="relative">
      <div class="absolute inset-y-0 start-0 flex items-center ps-3 pointer-events-none">
        <svg class="w-4 h-4 text-gray-500 dark:text-gray-400" aria-hidden="true" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 20 20">
          <path stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="m19 19-4-4m0-7A7 7 0 1 1 1 8a7 7 0 0 1 14 0Z"/>
        </svg>
      </div>
      <input v-model="searchTerm.query" @input="handleSearch" type="search" id="default-search"
             class="block w-full p-4 ps-10 text-sm text-gray-900 border border-gray-300 rounded-lg bg-gray-50 focus:ring-blue-500 focus:border-blue-500 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
             placeholder="Search for a city..." required/>
      <button type="submit"
              class="text-white absolute end-2.5 bottom-2.5 bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:outline-none focus:ring-blue-300 font-medium rounded-lg text-sm px-4 py-2 dark:bg-blue-600 dark:hover:bg-blue-700 dark:focus:ring-blue-800">
        Search
      </button>
    </div>
  </form>
  <div v-if="searchTerm.query != ''" class="bg-white my-2 rounded-lg shadow-lg mx-auto max-w-md">
    <div v-for="place in searchTerm.response" :key="place.id">
      <button class="px-3 my-2 hover: text-indigo-600 hover:font-bold w-full text-left" @click="searchForecast(place.id)">
        {{ place.name + ', ' + place.region + ', ' + place.country }}
      </button>
    </div>
  </div>
</template>