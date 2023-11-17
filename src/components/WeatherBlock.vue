<template>
  <div v-if="city">
    <div class="weather-card">
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
      <Button @click="addToFavourites">Add to Favourites</Button>
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
  </div>
</template>
  
  <script>
import Button from "./ui/Button.vue";
export default {
  props: ["city"],
  components: {
    Button,
  },
};
</script>

<style scoped>
.weather-card {
  background: #e5e5e5;
  padding: 20px;
  border-radius: 10px;
  max-width: 300px;
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
</style>