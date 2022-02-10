<template>
  <div class="card">
    <div class="flex-row">
      <!-- City name and weather description -->
      <div>
        <h2 class="card-header">{{ name }}</h2>
        <p class="card-description">{{ city.weather[0].description }}</p>
      </div>
      <!-- Weather icon and current temperature -->
      <div class="flex-row flex-center">
        <img class="card-img large" :src="`https://openweathermap.org/img/wn/` + city.weather[0].icon + `@2x.png`" />
        <p class="card-temperature">{{ this.roundTemperature(city.main.temp) }}</p>
      </div>
    </div>
    <div class="flex-row flex-end">
      <!-- Date and time -->
      <div>
        <p class="card-date">{{ this.dateConverter(city.dt) }}</p>
        <p class="card-time">{{ this.timeConverter(city.dt) }}</p>
      </div>
      <!-- Wind speen, humidity and precipitation-->
      <div>
        <ul class="card-list">
          <li>Wind: {{ city.wind.speed }} m/s</li>
          <li>Humidity: {{ city.main.humidity }} %</li>
          <!-- Show snow precipitation -->
          <li v-if="city.snow">Precipitation: {{ city.snow["1h"] || city.snow["3h"] }} mm</li>
          <!-- Show rain precipitation if there is no data for snow -->
          <li v-else-if="!city.snow && city.rain">Precipitation: {{ city.rain["1h"] || city.rain["3h"]}} mm</li>
          <!-- Precipitation default is 0 -->
          <li v-else>Precipitation: 0 mm</li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'WeatherCurrent',
  props: {
    city: Object,
    name: String
  },
  methods: {
    // Round temperature to the nearest whole number, e.g. 4.7 °C => 5 °C
    // If temperature is 0, remove minus -0 °C => 0 °C
    roundTemperature (temp) {
      if(temp >= (-0.5)) {
        return temp.toFixed(0).replace("-","") + ' °C'
      } else {
        return temp.toFixed(0) + ' °C'
      }
    },
    // Convert unix UTC time e.g. February 6th
    dateConverter (date) {
      const formatted = new Date(date * 1000).toLocaleString('en-US', {
        month: 'long',
        day: 'numeric'
      })
      const suffix = this.dateSuffix(formatted)
      return formatted + suffix
    },
    // Suffix for dates
    dateSuffix (num) {
      return ['st', 'nd', 'rd'] [((num + 90) % 100 - 10) % 10 - 1] || 'th'
    },
    // Convert time e.g. 16:30
    timeConverter (time) {
      const formatted = new Date(time * 1000).toLocaleTimeString('en-GB', {
        timeStyle: 'short'
      })
      return formatted
    }
  }
}
</script>

<style>
@import '../assets/main.css';
</style>
