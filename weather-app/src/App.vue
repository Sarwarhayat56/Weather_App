<template>
  <div class="app">
    <h1> Weather App</h1>
    <input v-model="city" placeholder="Enter city name" />
    <button @click="getWeather">Get Weather</button>

    <div v-if="weather.main">
      <h2>{{ weather.name }}, {{ weather.sys.country }}</h2>
      <p>  Temp: {{ weather.main.temp }}°C</p>
      <p>  Max: {{ weather.main.temp_max }}°C</p>
      <p>  Min: {{ weather.main.temp_min }}°C</p>
      <p> Condition: {{ weather.weather[0].main }}</p>
    </div>

    <p v-if="error" class="error">{{ error }}</p>
  </div>
</template>

<script setup> 
 import { ref } from 'vue';
import axios from 'axios';
const city = ref('');
const weather = ref({});
const error = ref('');

const API_KEY = '5d26b0fc31b45074765dd686c44a1c4e';

const getWeather = async () => {
  if (!city.value) {
    error.value = 'Please enter a city name';
    return;
  }

  try {
    const response = await axios.get(
      `https://api.openweathermap.org/data/2.5/weather?q=${city.value}&units=metric&appid=${API_KEY}`
    );
    weather.value = response.data;
    error.value = '';
    console.log("Weather data:", response.data);
  } catch (err) {
    console.error("API Error:", err);
    weather.value = {};
    error.value = 'City not found or API error';
  }
}; 
</script> 

<style scoped>
.app {
  max-width: 400px;
  margin: 50px auto;
  text-align: center;
  font-family: Arial, sans-serif;
}
input {
  padding: 10px;
  width: 80%;
  margin-bottom: 10px;
}
button {
  padding: 10px 20px;
  margin-bottom: 20px;
}
.error {
  color: red;
}
</style>

