<script setup>
import { ref } from "vue";

const weatherInfo = ref();
const cities = ref([]);

async function getCities() {
  const response = await fetch(
    "https://www.el-tiempo.net/api/json/v2/provincias/33/municipios"
  );
  const responseBody = await response.json();
  cities.value = responseBody.municipios.map((c) => ({
    name: c.NOMBRE,
    id: c.CODIGOINE.substring(0, 5),
  }));
}

async function getWeatherInformation(id) {
  const response = await fetch(
    `https://www.el-tiempo.net/api/json/v2/provincias/33/municipios/${id}`
  );
  const responseBody = await response.json();
  weatherInfo.value = {
    city: responseBody.municipio.NOMBRE,
    stateSky: responseBody.stateSky.description,
    temperature: responseBody.temperatura_actual,
  };
}

function updateWeather(e) {
  getWeatherInformation(e.target.value);
}

getWeatherInformation("33044");
getCities();
</script>

<template>
  <div class="weather">
    <div class="weather-card">
      <div class="text-center">
        {{ weatherInfo?.temperature }}ºC {{ weatherInfo?.city }} Sol
      </div>
      <span class="text-center">Cielo {{ weatherInfo?.stateSky }}</span>
      <label class="text-center ms-3"
        >Select a city
        <select class="" @change="updateWeather" value="33044">
          <option class="text-center" v-for="city in cities" :value="city.id">
            {{ city.name }}
          </option>
        </select>
      </label>
    </div>
  </div>
</template>

<style>
.weather {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  padding-left: 20px;
  padding-right: 20px;
  gap: 8px;
}

.weather-card {
  display: flex;
  width: 100%;
  flex-direction: column;
  border: 1px solid black;
  border-radius: 25px;
  padding: 16px;
  gap: 4px;
  background-color: rgb(0, 191, 255);
}
</style>
