<template>
  <div id="app">
    <!-- Header -->
    <Header />
    <!-- Content -->
    <div class="content">
      <!-- Select city -->
      <CityDropdownMenu :cities="cities" @clicked="getCities" />
      <!-- Show selected cities -->
      <div :key="refreshComponent">
        <div v-for="(city, index) in selectedCities" :key="index" class="weather-component">
          <CityWeather :selectedCity="city" :apiKey="apiKey" />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Header from './components/Header.vue'
import CityDropdownMenu from './components/CityDropdownMenu.vue'
import CityWeather from './components/CityWeather.vue'

export default {
  name: 'App',
  components: {
    Header,
    CityDropdownMenu,
    CityWeather
  },
  data () {
    return {
      apiKey: 'YOUR_API_KEY', // Use your own API key
      selectedCities: [],
      cities: [
        {'name': 'Kaikki kaupungit'},
        {'id': '660129', 'name': 'Espoo'},
        {'id': '655195', 'name': 'Jyväskylä'},
        {'id': '650225', 'name': 'Kuopio'},
        {'id': '634963', 'name': 'Tampere'}
      ],
      refreshComponent: 0
    }
  },
  mounted () {
    this.getCities(0)
  },
  methods: {
    // Get city from dropdown component
    getCities (selected) {
      this.selectedCities = []
      // Show all cities if 'Kaikki kaupungit' is selected
      if(selected === 0) {
        for(let i = 0; i < this.cities.length; i++) {
          this.selectedCities.push(this.cities[i])
        }
        // Remove first one, 'Kaikki kaupungit'
        this.selectedCities.shift()
      } else {
        this.selectedCities.push(this.cities[selected])
      }
      // Refresh component to see changes
      this.refreshComponent += 1
    }
  }
}
</script>

<style>
@import './assets/main.css';
</style>
