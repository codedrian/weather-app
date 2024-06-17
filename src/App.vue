<script setup>
import { ref } from 'vue'
import SearchRegion from './assets/components/SearchRegion.vue'
import DisplayForecast from './assets/components/DisplayForecast.vue'
const regionId = ref();
function displayForecast(id) {
	regionId.value = id;
}
</script>
<template>
	<SearchRegion @submit="displayForecast"/>
	<DisplayForecast :regionId="regionId"/>
</template>
