<script setup>
import {ref} from 'vue'
import SearchRegion from './assets/components/SearchRegion.vue'
import WeatherCard from './assets/components/WeatherCard.vue'

const places = ref([]);

function displayForecast(data) {
  places.value.push(data);
}

function deleteWeatherCard(name) {
    /*TODO: Delete the index of the name*/
    const weatherIndex = places.value.findIndex(place => place.location.name === name);
    if (weatherIndex !== -1) {
        places.value.splice(weatherIndex, 1); // Remove 1 element starting at weatherIndex
    }
    console.log(weatherIndex);
}

</script>
<template>
  <SearchRegion @place-data="displayForecast"/>
  <div class="grid grid-cols-2 gap-4 mt-5">
    <div v-for="(place, index) in places" :key="index">
      <WeatherCard :place="place" @delete-weather-card="deleteWeatherCard"/>
    </div>
  </div>
</template>
