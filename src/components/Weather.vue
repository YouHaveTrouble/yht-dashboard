<template>
<section class="weather">
  <i v-if="currentWeather" :class="`heading icon wi ${getWeatherIconFromWW(currentWeather.weathercode)}`"></i>
  <div v-if="currentWeather" class="temperature heading">{{ currentWeather.temperature }}°C</div>
</section>
</template>

<script lang="ts">
import { defineComponent } from 'vue'
import '../assets/weather/css/weather-icons.min.css'
export default defineComponent({
  name: "Weather",
  data() {
    return {
      currentWeather: null,
    }
  },
  methods: {
    updateData() {
      navigator.geolocation.getCurrentPosition((position) => {
        this.getWeatherData(position)
      });
    },
    async getWeatherData(geolocation: GeolocationPosition) {
      const latitude = geolocation.coords.latitude;
      const longitude = geolocation.coords.longitude;
      const response = await fetch(`https://api.open-meteo.com/v1/forecast?latitude=${latitude}&longitude=${longitude}&current_weather=true&timezone=auto`);
      if (response.status !== 200) {
        console.debug("Failed to update weather data, request failed.")
        return;
      }
      const json = await response.json();
      this.currentWeather= json.current_weather;
      console.debug("Updated weather data.");
    },
    isDay() {
      const hours = new Date().getHours()
      return hours > 6 && hours < 20
    },
    getWeatherIconFromWW(interpretationCode: Number): String {
      switch (interpretationCode) {
        default:
        case 0:
          return this.isDay() ? "wi-day-sunny" : "wi-night-clear";
        case 1:
        case 2:
        case 3:
          return this.isDay() ? "wi-day-cloudy" : "wi-night-alt-cloudy";
        case 45:
        case 48:
          return this.isDay() ? "wi-day-fog" : "wi-night-fog";
        case 51:
        case 53:
        case 55:
        case 56:
        case 57:
          return this.isDay() ? "wi-day-sprinkle" : "wi-night-alt-sprinkle";
        case 61:
        case 63:
        case 65:
        case 66:
        case 67:
          return this.isDay() ? "wi-day-rain" : "wi-night-alt-rain";
        case 71:
        case 73:
        case 75:
          return this.isDay() ? "wi-day-snow" : "wi-night-alt-snow";
        case 77:
          return this.isDay() ? "wi-day-hail" : "wi-night-alt-hail";
        case 80:
        case 81:
        case 82:
          return this.isDay() ? "wi-day-showers" : "wi-night-alt-showers";
        case 85:
        case 86:
          return this.isDay() ? "wi-day-snow-wind" : "wi-night-alt-snow-wind";
        case 95:
          return this.isDay() ? "wi-day-thunderstorm" : "wi-night-alt-thunderstorm";
        case 96:
        case 99:
          return this.isDay() ? "wi-day-storm-showers" : "wi-night-alt-storm-showers";
      }
    }
  },
  mounted() {
    this.updateData();
    setInterval(() => {
      this.updateData();
    }, 60_000)
  }
})
</script>

<style lang="scss" scoped>
.weather {
  display: flex;
  flex-direction: row;
  justify-content: center;
  gap: 1.5rem;
  text-align: center;
  .icon {
    transform: translateY(0.5rem);
  }
}

@media screen and (min-width:800px) {
  .weather {
    text-align: start;
  }
}
</style>