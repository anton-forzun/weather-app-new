<template>
  <div v-if="city !== undefined">
    <div class="btn_container">
      <Button @click="fetchWeatherData('today')">Сьогодні</Button>
      <Button @click="fetchWeatherData('week')">Тиждень</Button>
      <Button class="btn" @click="fetchWeatherData('day')"
        >День
        <img src="../assets/day.png" />
      </Button>
      <Button class="btn" @click="fetchWeatherData('night')"
        >Ніч <img src="../assets/night.png"
      /></Button>
    </div>

    <div class="weather-card" :class="{ selected: isSelected }">
      <div class="weather-card__location">
        <i class="weather-card__location-icon">
          <img
            v-if="city.weather && city.weather[0].icon"
            :src="getWeatherIconUrl(city.weather[0].icon)"
            alt="Weather Icon"
          />
        </i>
        <h2 class="weather-card__location-name">{{ city.name }}</h2>
      </div>
      <Button @click="addToFavourites">Додати в Обране</Button>
      <h1 v-if="city" class="weather-card__temp">
        {{ Math.floor(city.main.temp) }}&deg;C
      </h1>

      <div v-if="city" class="weather-card__desc">
        {{ city.weather[0].main }}
      </div>

      <div class="weather-card__info">
        <div class="weather-card__info-item">
          <span v-if="city" class="weather-card__info-label">Humidity:</span>
          {{ city.main.humidity }}%
        </div>
        <div class="weather-card__info-item">
          <span v-if="city" class="weather-card__info-label">Wind:</span>
          {{ city.wind.speed }}
        </div>
      </div>
    </div>

    <div class="weather-table" v-if="weatherType === 'week'">
      <h1>{{ weatherWeek.city.name }}</h1>
      <h3>country: {{ weatherWeek.city.country }}</h3>
      <p>timezone: {{ weatherWeek.city.timezone }}</p>
      <table>
        <thead>
          <tr>
            <th>День</th>
            <th>Час</th>
            <th>Опади</th>
            <th>Температура</th>
            <th>Вологість</th>
            <th>Вітер</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(day, index) in weatherWeek.list" :key="index">
            <td>{{ day.dt_txt.split(" ")[0] }}</td>
            <td>{{ day.dt_txt.split(" ")[1].slice(0, 5) }}</td>
            <td>
              <img :src="getWeatherIconUrl(day.weather[0].icon)" />
              <p>{{ day.weather[0].main }}</p>
            </td>
            <td>
              <p>max: {{ Math.floor(day.main.temp_max) }}&deg;C</p>
              min: {{ Math.floor(day.main.temp_min) }}&deg;C
            </td>
            <td>{{ day.main.humidity }}%</td>
            <td>{{ day.wind.speed }} m/s</td>
          </tr>
        </tbody>
      </table>
    </div>

    <div class="weather-table" v-if="weatherType === 'day'">
      <div class="day_container">
        <h1>День</h1>
        <img src="../assets/day.png" />
      </div>

      <table>
        <thead>
          <tr>
            <th>Число</th>
            <th>Час</th>
            <th>Опади</th>
            <th>Температура</th>
            <th>Вологість</th>
            <th>Вітер</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(day, index) in dayData" :key="index">
            <td>{{ day.dt_txt.split(" ")[0] }}</td>
            <td>{{ day.dt_txt.split(" ")[1].slice(0, 5) }}</td>
            <td>
              <img :src="getWeatherIconUrl(day.weather[0].icon)" />
              <p>{{ day.weather[0].main }}</p>
            </td>
            <td>
              <p>max: {{ Math.floor(day.main.temp_max) }}&deg;C</p>
              min: {{ Math.floor(day.main.temp_min) }}&deg;C
            </td>
            <td>{{ day.main.humidity }}%</td>
            <td>{{ day.wind.speed }} m/s</td>
          </tr>
        </tbody>
      </table>
    </div>
    <div class="weather-table" v-if="weatherType === 'night'">
      <div class="night_container">
        <h1>Ніч</h1>
        <img src="../assets/night.png" />
      </div>

      <table class="night">
        <thead>
          <tr>
            <th>Число</th>
            <th>Час</th>
            <th>Опади</th>
            <th>Температура</th>
            <th>Вологість</th>
            <th>Вітер</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(day, index) in nightData" :key="index">
            <td>{{ day.dt_txt.split(" ")[0] }}</td>
            <td>{{ day.dt_txt.split(" ")[1].slice(0, 5) }}</td>
            <td>
              <img :src="getWeatherIconUrl(day.weather[0].icon)" />
              <p>{{ day.weather[0].main }}</p>
            </td>
            <td>
              <p>max: {{ Math.floor(day.main.temp_max) }}&deg;C</p>
              min: {{ Math.floor(day.main.temp_min) }}&deg;C
            </td>
            <td>{{ day.main.humidity }}%</td>
            <td>{{ day.wind.speed }} m/s</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import Button from "./ui/Button.vue";
export default {
  props: ["city"],
  data() {
    return {
      weatherDay: [],
      weatherWeek: [],
      isDayMode: true,
      weatherType: "",
      dayData: [],
      nightData: [],
      ApiKey: "e59f52e2ed19fa4c33d87cfa56cb3f5a",
      isSelected: false,
    };
  },
  methods: {
    addToFavourites() {
      this.isSelected = !this.isSelected;
      this.$emit("add-to-favourites", this.city);
    },
    getWeatherIconUrl(iconCode) {
      return `http://openweathermap.org/img/wn/${iconCode}.png`;
    },
    removeWeatherCard() {
      this.$emit("remove-card", this.city.id);
    },
    async fetchWeatherData(type) {
      try {
        // Логіка для відображення погоди на день
        if (type === "today") {
          const res = await axios.get(
            `https://api.openweathermap.org/data/2.5/weather?q=${this.city.name}&appid=${this.ApiKey}&units=metric`
          );
          this.weatherDay = res.data;
          this.weatherType = type;
        }
        if (type === "week") {
          // Логіка для відображення погоди на тиждень
          const res = await axios.get(
            `https://api.openweathermap.org/data/2.5/forecast?q=${this.city.name}&appid=${this.ApiKey}&units=metric`
          );
          this.weatherWeek = res.data;
          this.weatherType = type;
        }
        if (type === "day") {
          // Логіка для відображення погоди на день
          const res = await axios.get(
            `https://api.openweathermap.org/data/2.5/forecast?q=${this.city.name}&appid=${this.ApiKey}&units=metric`
          );
          this.weatherWeek = res.data;
          this.weatherType = type;
          this.dayData = this.weatherWeek.list.filter(
            (item) => item.sys.pod === "d"
          );
        }
        if (type === "night") {
          // Логіка для відображення погоди на ніч
          const res = await axios.get(
            `https://api.openweathermap.org/data/2.5/forecast?q=${this.city.name}&appid=${this.ApiKey}&units=metric`
          );
          this.weatherWeek = res.data;
          this.weatherType = type;
          this.nightData = this.weatherWeek.list.filter(
            (item) => item.sys.pod === "n"
          );
        }
      } catch (error) {
        console.error("Error fetching weather data:", error);
      }
    },
  },
  components: {
    Button,
  },
};
</script>

<style scoped>
.day_container {
  margin: 20px 0;
  display: flex;
  gap: 20px;
}
.day_container img {
  width: 50px;
}
.night_container {
  margin: 20px 0;
  display: flex;
  gap: 20px;
}
.night_container img {
  width: 50px;
}
.selected {
  border: 2px solid rgb(63, 35, 163);
}
.weather-card {
  background: #e5e5e5;
  padding: 20px;
  border-radius: 10px;
  max-width: 300px;
  -webkit-box-shadow: -1px 2px 36px -6px #616161;
  box-shadow: -1px 2px 36px -6px #616161;
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
.weather-table {
  max-width: 100%;
  overflow-y: auto;
  max-height: 400px;
}

table {
  width: 100%;
  border-collapse: collapse;
  margin-top: 20px;
  position: relative;
}
thead {
  position: sticky;
  top: 0;
  z-index: 1;
}

th,
td {
  border: 1px solid #ddd;
  padding: 8px;
  text-align: left;
}

th {
  background-color: #f2f2f2;
}

.btn_container {
  display: flex;
  flex-wrap: wrap;
  margin: 20px 0;
}

.btn {
  display: flex;
  align-items: center;
}
.btn img {
  width: 20px;
  height: 20px;
  margin: auto;
}
.night {
  background: rgba(0, 0, 0, 0.5);
  color: white;
}
.night thead {
  color: #616161;
}
</style>
