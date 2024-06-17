<script setup>
import { ref } from 'vue';

  const weatherInfo = ref()
  const cities = ref([])

  async function getCities() {
    const response = await fetch('https://www.el-tiempo.net/api/json/v2/provincias/33/municipios')
    const responseBody = await response.json()
    cities.value = responseBody.municipios.map(c => ({
      name: c.NOMBRE,
      id: c.CODIGOINE.substring(0, 5)
    }))
  }

  async function getWeatherInformation(id) {
    const response = await fetch(`https://www.el-tiempo.net/api/json/v2/provincias/33/municipios/${id}`)
    const responseBody = await response.json()
    weatherInfo.value = {
      city: responseBody.municipio.NOMBRE,
      stateSky: responseBody.stateSky.description,
      temperature: responseBody.temperatura_actual
    }
  }

  function updateWeather(e) {
    getWeatherInformation(e.target.value)
  }

  getWeatherInformation('33044')
  getCities()
</script>

<template>
    <div class="about">
      <h1>Weather</h1>
      <label>Select a city
        <select @change="updateWeather" value="33044">
          <option v-for="city in cities" :value="city.id">{{ city.name }}</option>
        </select>
      </label>
      <div class="weather-card">
        <div>{{ weatherInfo.temperature }}ºC {{ weatherInfo.city }} Solecito</div>
        <span>Cielo {{ weatherInfo.stateSky }}</span>
      </div>
    </div>
  </template>
  
  <style>
  @media (min-width: 1024px) {
    .about {
      min-height: 100vh;
      display: flex;
      flex-direction: column;
      align-items: center;
    }

    .weather-card {
      display: flex;
      flex-direction: column;
      border: 1px solid black;
      border-radius: 25px;
      padding: 16px;
      gap: 4px;
      background-color: rgb(0,191,255);
    }
  }
  </style>
  