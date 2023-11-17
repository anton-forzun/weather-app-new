
<script setup>
import { RouterLink, RouterView } from "vue-router";
</script>

<template>
  <div>
    <div class="nav_container">
      <h2>MAIN</h2>
      <button
        class="btn5"
        @click="openFavourites"
        :disabled="isFavouritesEmpty"
      >
        Обране
        <!-- <span class="quontity" v-if="this.select">{{
          this.select.length
        }}</span> -->
      </button>
    </div>
    <CityInput @city-selected="addWeatherBlock" />
    <WeatherMap :city="selectedCity" @add-to-favourites="addToFavourites" />
    <TemperatureChart
      v-if="selectedCity.temperatureData"
      :temperatureData="selectedCity.temperatureData"
    />

    <div v-if="show" class="popup-overlay">
      <div v-if="select" class="popup">
        <h2>Обране</h2>
        <button class="remove_btn" @click="closeFav">X</button>
        <div class="container">
          <ul>
            <li v-for="city in select" :key="city.id">
              <div class="weather-card" @click="selectfavoriteCard(city)">
                <button class="remove_btn" @click="showConfirmation(city)">
                  X
                </button>
                <div class="weather-card__location">
                  <i class="weather-card__location-icon">
                    <img
                      v-if="city.weather && city.weather[0].icon"
                      :src="getWeatherIconUrl(city.weather[0].icon)"
                      alt="Weather Icon"
                    />
                  </i>
                  <h2
                    class="weather-card__location-name"
                    :class="{ selected: isSelected }"
                  >
                    {{ city.name }}
                  </h2>
                </div>
                <h1 class="weather-card__temp">{{ city.main.temp }}&deg;C</h1>

                <div class="weather-card__desc">{{ city.weather[0].main }}</div>

                <div class="weather-card__info">
                  <div class="weather-card__info-item">
                    <span class="weather-card__info-label">Humidity:</span>
                    {{ city.main.humidity }}%
                  </div>
                  <div class="weather-card__info-item">
                    <span class="weather-card__info-label">Wind:</span>
                    {{ city.wind.speed }}
                  </div>
                </div>
              </div>
            </li>
          </ul>
        </div>

        <h3 v-if="!this.select">Нажаль нічого не додано...</h3>
      </div>
    </div>

    <ConfirmationPopup
      :visible="isConfirmationVisible"
      @close-modal="closeConfirmation"
      @confirm-action="performAction"
    />
    <Modal :visible="isModalVisible" @closeModal="closeMod" />
  </div>

  <RouterView />
</template>

<script>
import CityInput from "../components/CityInput.vue";
import WeatherMap from "../components/WeatherMap.vue";
// import WeatherBlock from "../components/WeatherBlock.vue";
import TemperatureChart from "../components/TemperatureChart.vue";
import Modal from "../components/Modal.vue";
import ConfirmationPopup from "./ConfirmationPopup.vue";

export default {
  data() {
    return {
      selectedCity: JSON.parse(localStorage.getItem("selectedCity")) || [],
      select: JSON.parse(localStorage.getItem("favouriteCities")) || [],
      show: false,
      weatherBlocks: JSON.parse(localStorage.getItem("weatherBlocks")) || [],
      isModalVisible: false,
      isConfirmationVisible: false,
    };
  },
  mounted() {},
  computed: {
    isFavouritesEmpty() {
      return this.select.length === 0;
    },
  },
  methods: {
    showConfirmation(city) {
      this.cityToRemove = city;
      this.isConfirmationVisible = true;
    },
    closeConfirmation() {
      this.cityToRemove = null;
      this.isConfirmationVisible = false;
    },
    performAction(action) {
      if (action === "confirm") {
        this.removeFavourite(this.cityToRemove);
      }
      this.closeConfirmation();
    },

    selectfavoriteCard(city) {
      console.log(city);
      this.fetchTemperatureData(city.name)
        .then((temperatureData) => {
          this.selectedCity = { ...city, temperatureData };

          this.saveDataToLocal();
        })
        .catch((error) => {
          console.error("Error fetching temperature data:", error);
        });
    },
    closeMod(e) {
      this.isModalVisible = e;
    },
    removeFavourite(city) {
      this.select = this.select.filter(
        (selectedCity) => selectedCity.id !== city.id
      );
      if (!this.select.length === 0) {
        this.show = false;
      }
      this.saveDataToLocal();
    },
    closeFav() {
      this.show = false;
    },
    openFavourites() {
      this.show = true;
    },
    async fetchTemperatureData(cityName) {
      const apiKey = "e59f52e2ed19fa4c33d87cfa56cb3f5a";
      const response = await fetch(
        `https://api.openweathermap.org/data/2.5/forecast?q=${cityName}&appid=${apiKey}&units=metric`
      );
      const data = await response.json();

      const hourlyTemperatureData = data.list.map((item) => ({
        time: new Date(item.dt * 1000).getHours(),
        temperature: item.main.temp,
      }));

      return hourlyTemperatureData;
    },

    async addWeatherBlock(city) {
      this.fetchTemperatureData(city.name)
        .then((temperatureData) => {
          this.selectedCity = { ...city, temperatureData };
          this.saveDataToLocal();
        })
        .catch((error) => {
          console.error("Error fetching temperature data:", error);
        });
    },
    addToFavourites(city) {
      const isAlreadyInFavourites = this.select.some(
        (selectedCity) => selectedCity.id === city.id
      );
      if (this.select.length >= 5) {
        this.isModalVisible = true;
      } else if (!isAlreadyInFavourites && this.select.length < 5) {
        this.select.push(city);
        console.log(this.select.length);
        this.saveDataToLocal();
      } else {
        console.warn("City is already in favourites!");
      }
    },
    getWeatherIconUrl(iconCode) {
      return `https://openweathermap.org/img/wn/${iconCode}.png`;
    },
    saveDataToLocal() {
      localStorage.setItem("selectedCity", JSON.stringify(this.selectedCity));
      localStorage.setItem("weatherBlocks", JSON.stringify(this.weatherBlocks));
      localStorage.setItem("favouriteCities", JSON.stringify(this.select));
    },
  },
  components: {
    CityInput,
    WeatherMap,
    // WeatherBlock,
    Modal,
    RouterLink,
    RouterView,
    TemperatureChart,
    ConfirmationPopup,
  },
};
</script>

<style scoped>
.nav_container {
  display: flex;
  justify-content: space-between;
}
.container {
  overflow-y: auto;
}
ul {
  display: flex;
  gap: 20px;
  list-style-type: none;
  width: max-content;
  padding: 20px 0;
}
li {
  cursor: pointer;
}
.popup-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;

  background: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1000;
}

.popup {
  width: 100%;
  background: #fff;
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.2);
  position: relative;
}

header {
  display: flex;
  justify-content: center;
  flex-direction: column;
  align-items: center;
}
.logo {
  max-width: 200px;
  width: 100%;
}
.weather-card {
  background: #e5e5e5;
  padding: 20px;
  border-radius: 10px;
  max-width: 200px;
  position: relative;
  transition: 0.3s ease, box-shadow 0.3s ease;
}
.weather-card:hover {
  top: -2px;
  -webkit-box-shadow: 5px 5px 15px -4px #5e5e5e;
  box-shadow: 5px 5px 15px -4px #5e5e5e;
}
.remove_btn {
  display: inline-block;
  box-sizing: border-box;
  padding: 0 15px;
  right: -13px;
  top: -18px;
  outline: none;
  border: none;
  border-radius: 50%;
  height: 40px;
  line-height: 40px;
  font-size: 17px;
  font-weight: 600;
  text-decoration: none;
  color: #385898;
  background-color: #e7f3ff;
  cursor: pointer;
  user-select: none;
  appearance: none;
  touch-action: manipulation;
  position: absolute;
  z-index: 10;
}
.remove_btn:focus-visible {
  box-shadow: 0 0 0 2px #666;
}
.remove_btn:hover {
  background-color: #dbe7f2;
}
.remove_btn:active {
  transform: scale(0.96);
}
.remove_btn:disabled {
  pointer-events: none;
  opacity: 0.65;
}

.weather-card__location {
  display: flex;
  align-items: center;
}

.weather-card__location-icon {
  font-size: 30px;
  margin-right: 10px;
}

.weather-card__location-name {
  font-size: 1.2em;
  font-weight: bold;
}

.weather-card__temp {
  font-size: 2.5em;
  font-weight: bold;
  margin: 10px 0;
}

.weather-card__desc {
  text-transform: capitalize;
  margin-bottom: 10px;
}

.weather-card__info {
  display: flex;
  justify-content: space-between;
}

.weather-card__info-label {
  font-weight: bold;
}
.btn5 {
  display: inline-block;
  box-sizing: border-box;
  padding: 0 20px;
  margin: 0 15px 15px 0;
  outline: none;
  border: none;
  border-radius: 6px;
  height: 40px;
  line-height: 40px;
  font-size: 17px;
  font-weight: 600;
  text-decoration: none;
  color: #385898;
  background-color: #e7f3ff;
  cursor: pointer;
  user-select: none;
  appearance: none;
  touch-action: manipulation;
  position: relative;
}
.quontity {
  position: absolute;
  display: flex;
  align-items: center;
  justify-content: center;
  top: -7px;
  right: -9px;
  background: blue;
  color: white;
  border-radius: 50%;
  font-size: 12px;
  font-weight: 500;
  width: 20px;
  height: 20px;
}
.btn5:focus-visible {
  box-shadow: 0 0 0 2px #666;
}
.btn5:hover {
  background-color: #dbe7f2;
}
.btn5:active {
  transform: scale(0.96);
}
.btn5:disabled {
  pointer-events: none;
  opacity: 0.65;
}
</style>
