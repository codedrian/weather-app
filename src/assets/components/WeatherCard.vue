<script setup>
import BorderLine from './BorderLine.vue'
import WeatherForecastDay from './WeatherForecastDay.vue'
import WeatherInfo from "./WeatherInfo.vue";
import {ref} from "vue";
defineProps({
    place: Object
})
const isVisible = ref(false);
</script>
<template>
<!--If day/night change bg-->
  <div class="text-white p-10 rounded-lg shadow-lg gap-6 mb-6 relative overflow-hidden" :class="place.current.is_day ? 'bg-day' : 'bg-night'">
    <!-- Location & time -->
    <div class="mb-2 flex justify-between items-center">
      <div class="flex items-center justify-center gap-2">
        <i class="fa-solid fa-location-dot"></i>
        <h1 class="text-3xl">{{ place.location.name }}</h1>
      </div>
      <div class="flex items-center justify-center gap-2">
        <i class="fa-solid fa-clock"></i>
        <h1 class="text-3xl">{{ new Date(place.location.localtime).getHours() }}:{{ new Date(place.location.localtime).getMinutes() }}</h1>
      </div>
    </div>

    <!-- current weather -->
    <div class="text-center flex-1">
      <img :src="place.current.condition.icon" alt="icon" width="200" class="mx-auto -mb-10" />
      <h1 class="text-9xl mb-2">{{ place.current.temp_c }}&deg</h1>
      <p class="text-2xl">{{ (place.current.condition.text) }}</p>
    </div>

    <BorderLine />

    <!-- forecast -->
    <div v-for="(day, index) in place.forecast.forecastday" id="index">
      <WeatherForecastDay :day="day"/>
    </div>

    <!-- info -->
    <div>
        <Transition name="weather-info-fade">
            <WeatherInfo :info="place.current" v-show="isVisible" @close-weather-info="() => isVisible = false"
                         @delete-weather-card="$emit('delete-weather-card', place.location.name)"/>
        </Transition>
    </div>

    <!-- forecast btn -->
    <div class="flex justify-end items-center gap-1 mt-10">
      <button @click="() => isVisible = true">More <i class="fa-solid fa-arrow-right text-sm -mb-px"></i></button>
    </div>
  </div>
</template>
<style scoped>
.weather-info-fade-enter-active {
    opacity: 0;
    transition: opacity 0.3s ease-in-out;
}

.weather-info-fade-leave-active {
    opacity: 0;
    transition: opacity 0.3s ease-in-out;
}
.bg-day {
    background-color: #8ec5fc;
    background-image: linear-gradient(62deg, #8ec5fc 0%, #e0c3fc 100%);
}
.bg-night {
    background-color: #07223d;
    background-image: linear-gradient(62deg, #0a2a4a 0%, #270845 100%);
}
</style>