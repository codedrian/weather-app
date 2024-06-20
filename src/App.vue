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
	<div class="grid grid-cols-2 gap-4">
		<div v-for="(place, index) in places" :key="index">
			<WeatherCard :place="place" />
		</div>
	</div>
</template>
