<template>
  <div class="app">
    <h1> Weather App</h1>
    <input v-model="city" placeholder="Enter city name" />
    <div class="btns">
      <button @click="getWeather">Get Weather</button>
      <button @click="showPrevious">Previous Day</button>
      <button @click="showNext">Next Day</button>
      <button @click="showAnalytics">Analytics</button>
    </div>
    <!-- updated -->
    <WeatherToday v-if="active === 'today' && weatherToday" :weather="weatherToday" />
    <WeatherPrevious v-if="active === 'previous' && weatherPrevious" :weather="weatherPrevious" />
    <WeatherNext v-if="active === 'next' && weatherNext" :weather="weatherNext" />
    <WeatherAnalytics v-if="active === 'analytics' && weatherToday && forecastList" :weather="weatherToday"
      :forecast="forecastList" />
    <p v-if="error" class="error">{{ error }}</p>
  </div>
</template>

<script setup>
import { ref } from 'vue';
import axios from 'axios';

import WeatherToday from './components/WeatherToday.vue';
import WeatherPrevious from './components/WeatherPrevious.vue';
import WeatherNext from './components/WeatherNext.vue';
import WeatherAnalytics from './components/WeatherAnalytics.vue';

const city = ref('')
const weatherToday = ref(null)
const weatherPrevious = ref(null)
const weatherNext = ref(null)
const forecastList = ref([]) // for analytics
const error = ref('')
const active = ref('today')

const API_KEY = '5d26b0fc31b45074765dd686c44a1c4e';
const getWeather = async () => {
  if (!city.value) {
    error.value = 'Please enter a city name';
    return;
  }
  try {
    const todayRes = await axios.get(
      `https://api.openweathermap.org/data/2.5/weather?q=${city.value}&units=metric&appid=${API_KEY}`
    )

    const forecastRes = await axios.get(
      `https://api.openweathermap.org/data/2.5/forecast?q=${city.value}&units=metric&appid=${API_KEY}`
    )
    const nextDay = forecastRes.data.list.find(item =>
      item.dt_txt.includes('12:00:00')
    );
    weatherToday.value = todayRes.data; forecastList.value = forecastRes.data.list;
    weatherNext.value = nextDay;

    weatherPrevious.value = {
      name: todayRes.data.name,
      sys: todayRes.data.sys,
      main: {
        temp: todayRes.data.main.temp - 2,
        temp_max: todayRes.data.main.temp_max - 1,
        temp_min: todayRes.data.main.temp_min - 2,
      },
      weather: [{ main: 'Clouds' }]
    }
    active.value = 'today';
    error.value = '';
    // previous 
    // weather.value = response.data;
    // error.value = '';
    // console.log("Weather data:", response.data);
  }
  catch (err) {
    console.error("API Error:", err);
    // weather.value = {};
    error.value = 'City not found or API error';
  }
};
const showPrevious = () => {
  active.value = 'previous';
}
const showNext = () => {
  active.value = 'next';
}
const showAnalytics = () => {
  active.value = 'analytics';
}
</script>

<style scoped>
.app {
  max-width: 500px;
  margin: 30px auto;
  text-align: center;
  font-family: Arial, sans-serif;
}

input {
  padding: 10px;
  width: 80%;
  margin-bottom: 10px;
}

.btns {
  margin: 10px 0;
}

button {
  padding: 10px 15px;
  /* margin-bottom: 20px; */
  margin: 5px;
  cursor: pointer;
}

.error {
  color: red;
}
</style>
