<script setup>
import { ref } from 'vue'
import SearchRegion from './assets/components/SearchRegion.vue'
import WeatherCard from './assets/components/WeatherCard.vue'
const places = ref([]);
function displayForecast(data) {
	places.value.push(data);
}

</script>
<template>
	<SearchRegion @place-data="displayForecast"/>
	<div v-for="place in places">
		<WeatherCard :place="place" />
	</div>
</template>
