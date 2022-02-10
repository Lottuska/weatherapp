<template>
  <div>
    <!-- Current weather -->
    <WeatherCurrent v-if="cityData" :city="cityData" :name="selectedCity.name" />
    <!-- Weather forecast every three hours -->
    <WeatherForecast v-if="cityData" :city="cityData" :apiKey="apiKey"/>
  </div>
</template>

<script>
import WeatherCurrent from './WeatherCurrent.vue'
import WeatherForecast from './WeatherForecast.vue'
import axios from 'axios'

export default {
  name: 'CityWeather',
  components: {
    WeatherCurrent,
    WeatherForecast
  },
  props: {
    selectedCity: Object,
    apiKey: String
  },
  data () {
    return {
      apiURL1: 'https://api.openweathermap.org/data/2.5/weather?id=',
      apiURL2: '&units=metric&lang=en&appid=',
      cityID: this.selectedCity.id,
      cityData: '',
    }
  },
  async mounted () {
    this.getWeatherData()
  },
  methods: {
    // Get current weather data for selected city
    getWeatherData () {
      try {
        axios
          .get(this.apiURL1 + this.cityID + this.apiURL2 + this.apiKey)
          .then(response => (this.cityData = response.data))
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
    }
  }
}
</script>
