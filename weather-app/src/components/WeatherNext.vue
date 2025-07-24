<template>
  <div class="card">
    <h2>Next Day - {{ weather.name }}</h2>
    <p>Expected Temp: {{ weather.main.temp }}°C</p>
    <p>Max: {{ weather.main.temp_max }}°C</p>
    <p>Min: {{ weather.main.temp_min }}°C</p>
    <p>Condition: {{ weather.weather[0].main }}</p>
  </div>
</template>

<script setup>
defineProps(['weather'])
</script>

<style scoped>
.card {
  background-color: #000608;
  padding: 15px;
  border-radius: 10px;
  margin-top: 15px;
}
</style>
