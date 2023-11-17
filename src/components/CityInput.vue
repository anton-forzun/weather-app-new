<template>
  <div>
    <div class="autocomplete-container">
      <input
        v-model="city"
        @input="searchCities"
        placeholder="Search for a city..."
        class="autocomplete-input"
      />
      <ul v-if="showResults" class="autocomplete-results">
        <li
          v-for="result in results"
          :key="result.id"
          @click="selectCity(result.name)"
          class="autocomplete-result-item"
        >
          {{ result.name }}
        </li>
      </ul>
    </div>
  </div>
</template>
  
  <script>
import axios from "axios";

export default {
  data() {
    return {
      ApiKey: "e59f52e2ed19fa4c33d87cfa56cb3f5a",
      city: "",
      results: [],
      result2: [],
      locationIP: "",
      showResults: false,
      token: "f5dc12e372d1c3",
    };
  },
  mounted() {
    this.geolocationWeather();
  },
  methods: {
    async geolocationWeather() {
      try {
        const response = await axios.get(
          `https://ipinfo.io/json?token=${this.token}`
        );
        this.locationIP = response.data.city;
        console.log(this.locationIP);

        this.selectCity(this.locationIP);
      } catch (error) {
        console.error("Error fetching weather data:", error);
      }
    },
    async searchCities() {
      try {
        const response = await axios.get(
          `http://api.openweathermap.org/geo/1.0/direct?q=${this.city}&limit=5&appid=${this.ApiKey}`
        );
        this.results = response.data;
        this.showResults = true;
      } catch (error) {
        console.error("Error fetching city data:", error);
      }
    },
    async selectCity(city) {
      console.log(city);
      const res = await axios.get(
        `https://api.openweathermap.org/data/2.5/weather?q=${city}&appid=${this.ApiKey}&units=metric`
      );
      this.result2 = res.data;
      this.$emit("city-selected", this.result2);
      this.showResults = false;
    },
  },
};
</script>

<style scoped>
.autocomplete-container {
  position: relative;
  width: 300px;
}
.autocomplete-input {
  width: 100%;
  padding: 10px;
  font-size: 16px;
  border: 1px solid #ccc;
  border-radius: 5px;
  outline: none;
}

.autocomplete-results {
  list-style: none;
  margin: 0;
  padding: 0;
  position: absolute;
  top: 100%;
  left: 0;
  width: 100%;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  border: 1px solid #ccc;
  border-radius: 5px;
  background-color: #fff;
  z-index: 10;
}

.autocomplete-result-item {
  padding: 10px;
  cursor: pointer;
  transition: background-color 0.3s;
}

.autocomplete-result-item:hover {
  background-color: #f0f0f0;
}
</style>