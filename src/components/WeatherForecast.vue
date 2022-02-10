<template>
  <div class="flex-row">
    <div class="card without-border flex-col" v-for="(forecast, index) in weatherForecast.slice(1,6)" :key="index">
      <!-- Time, weather icon and temperature -->
      <div class="card-top">
        <p class="card-time">{{ timeConverter(forecast.dt) }}</p>
        <img class="card-img" :src="`https://openweathermap.org/img/wn/` + forecast.weather[0].icon + `@2x.png`" />
        <p class="card-temperature small">{{ roundTemperature(forecast.main.temp) }}</p>
      </div>
      <!-- Wind speed, humidity and precipitation -->
      <div class="card-bottom">
        <ul class="card-list small">
          <li>{{ forecast.wind.speed }} m/s</li>
          <li>{{ forecast.main.humidity }} %</li>
          <!-- Show snow precipitation -->
          <li v-if="forecast.snow">{{ forecast.snow["1h"] || forecast.snow["3h"] }} mm</li>
          <!-- Show rain precipitation if there is no data for snow -->
          <li v-else-if="!forecast.snow && forecast.rain">{{ forecast.rain["1h"] || forecast.rain["3h"]}} mm</li>
          <!-- Precipitation default is 0 -->
          <li v-else>0 mm</li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'WeatherForecast',
  props: {
    city: {},
    apiKey: String
  },
  data () {
    return {
      apiURL1: 'https://api.openweathermap.org/data/2.5/forecast?id=',
      apiURL2: '&units=metric&appid=',
      cityID: this.city.id,
      api: this.apiKey,
      weatherForecast: []
    }
  },
  mounted () {
    this.getWeatherForecast()
  },
  methods: {
    // Get 5 day / 3 hour forecast data for selected city
    getWeatherForecast () {
      try {
        axios
          .get(this.apiURL1 + this.cityID + this.apiURL2 + this.api)
          .then(response => (this.weatherForecast = response.data.list))
          .catch(function (error) {
            if(error.response) {
              console.log(error.response.data)
              console.log(error.response.status)
              console.log(error.response.headers)
            } else if(error.request) {
              console.log(error.request)
            } else {
              console.log('Error', error.message)
            }
          })
      } catch(error){
        console.log(error)
      }
    },
    // Round temperature to the nearest whole number, e.g. 4.7 °C => 5 °C
    // If temperature is 0, remove minus -0 °C => 0 °C
    roundTemperature (temp) {
      if(temp >= (-0.5)) {
        return temp.toFixed(0).replace("-","") + ' °C'
      } else {
        return temp.toFixed(0) + ' °C'
      }
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
