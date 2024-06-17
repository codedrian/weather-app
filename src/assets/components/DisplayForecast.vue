<script setup>

const props = defineProps({
    regionId: {
        type: Number,
        required: true
    }
})
</script>
<template>
    <p>The region ID is: {{ regionId }}</p>
</template>
