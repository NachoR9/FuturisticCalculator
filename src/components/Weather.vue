<script setup>
import { ref, computed } from "vue";

const weatherInfo = ref();
const cities = ref([]);
const weatherClass = ref("");

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
  updateWeatherClass(weatherInfo.value.stateSky);
}

function updateWeatherClass(stateSky) {
  if (stateSky.includes("sol")) {
    weatherClass.value = "sunny";
  } else if (stateSky.includes("lluvia")) {
    weatherClass.value = "rainy";
  } else if (stateSky.includes("tormenta")) {
    weatherClass.value = "storm";
  } else if (stateSky.includes("cubierto")) {
    weatherClass.value = "covered";
  } else if (stateSky.includes("niebla")) {
    weatherClass.value = "fog";
  } else if (stateSky.includes("poco nuboso")) {
    weatherClass.value = "kinda-cloudy";
  } else if (stateSky.includes("nuboso")) {
    weatherClass.value = "cloudy";
  } else {
    weatherClass.value = "";
  }
}

function updateWeather(e) {
  getWeatherInformation(e.target.value);
}

getWeatherInformation("33044");
getCities();
</script>

<template>
  <div :class="['weather', weatherClass]">
    <div class="weather mr-5">
      <div class="weather-box">
        <div class="text-center">
          {{ weatherInfo?.temperature }}ºC {{ weatherInfo?.city }}
        </div>
        <span class="text-center">Cielo {{ weatherInfo?.stateSky }}</span>
        <label class="text-center ms-3">
          Select a city
          <select class="" @change="updateWeather" value="33044">
            <option class="text-center" v-for="city in cities" :value="city.id">
              {{ city.name }}
            </option>
          </select>
        </label>
      </div>
    </div>
  </div>
</template>

<style lang="scss" scoped>
.sunny {
  background: url("https://fotografias.lasexta.com/clipping/cmsimages02/2022/04/29/77BCFF9C-7D8B-474B-A45F-E951270DE9E5/buena-temperatura-sol_98.jpg?crop=2121,1193,x0,y62&width=1900&height=1069&optimize=high&format=webply")
    no-repeat center center;
  background-size: cover;
}

.rainy {
  background: url("https://i.pinimg.com/736x/37/5e/6e/375e6e01c814dd075c8466313b40d79d.jpg")
    no-repeat center center;
  background-size: cover;
}

.cloudy {
  background: url("https://i.pinimg.com/originals/6f/4e/87/6f4e87c57bad8495f4dd2ae9d5e24277.jpg")
    no-repeat center center;
  background-size: cover;
}

.covered {
  background: url("https://i.pinimg.com/originals/6f/4e/87/6f4e87c57bad8495f4dd2ae9d5e24277.jpg")
    no-repeat center center;
  background-size: cover;
}

.kinda-cloudy {
  background: url("https://i.pinimg.com/originals/6f/4e/87/6f4e87c57bad8495f4dd2ae9d5e24277.jpg")
    no-repeat center center;
  background-size: cover;
}

.fog {
  background: url("https://i.pinimg.com/originals/6f/4e/87/6f4e87c57bad8495f4dd2ae9d5e24277.jpg")
    no-repeat center center;
  background-size: cover;
}

.storm {
  background: url("https://t4.ftcdn.net/jpg/04/15/75/37/360_F_415753793_Amq32dxFXnCvub1rJ3UbCYjyzV7CdAce.jpg")
    no-repeat center center;
  background-size: cover;
}

.weather {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding-top: 25px;
  padding-bottom: 25px;
  padding-left: 25px;
  padding-right: 25px;
  gap: 8px;
  color: black;
  font-size: 10px;
  background-color: rgb(255, 255, 255);
}

.weather-box {
  background-color: rgb(255, 255, 255);
}

.weather-card {
  display: flex;
  max-width: 420px;
  flex-direction: column;
  border: 1px solid black;
  border-radius: 25px;
  padding: 16px;
  gap: 4px;
  background-color: rgb(0, 191, 255);
  color: black;
}
</style>
