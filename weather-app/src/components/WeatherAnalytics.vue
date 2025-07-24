<template>
  <div class="analytics">
    <h2>Weather Analytics for {{ weather.name }}</h2>

    <!-- Basic Weather Stats -->
    <div class="stats">
      <p> Feels Like: {{ weather.main.feels_like }}°C</p>
      <p> Humidity: {{ weather.main.humidity }}%</p>
      <p> Wind Speed: {{ weather.wind.speed }} m/s</p>
      <p> Cloudiness: {{ weather.clouds.all }}%</p>
      <p> Pressure: {{ weather.main.pressure }} hPa</p>
    </div>

     <!-- Forecast Trend -->
    <div class="trend">
      <h3>📈 Temperature Prediction (Next 24h)</h3>
      <ul>
        <li v-for="(item, index) in forecast.slice(0, 8)" :key="index">
          {{ formatTime(item.dt_txt) }} - {{ item.main.temp }}°C ({{ item.weather[0].main }})
        </li>
      </ul>
    </div>
  </div>
</template>

<script setup>
defineProps(['weather', 'forecast'])

const formatTime = (dt) => {
  return new Date(dt).toLocaleTimeString(undefined, {
    hour: '2-digit',
    minute: '2-digit',
  })
}
</script>

<style scoped>
.analytics {
  background: #080808;
  padding: 20px;
  border-radius: 12px;
  margin-top: 15px;
  text-align: left;
}
.stats p {
  margin: 6px 0;
}
.trend ul {
  list-style: none;
  padding: 0;
}
.trend li {
  margin: 4px 0;
}
</style>
