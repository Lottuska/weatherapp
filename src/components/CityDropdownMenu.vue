<template>
  <!-- Show all available options in dropdown -->
  <select name="cities" id="cities" class="card dropdown" v-model="selected" v-on:change="selectCity">
    <option v-for="(city, index) in cities" :key="index" :value="index">{{ city.name }}</option>
  </select>
</template>

<script>
export default {
  name: 'CityDropdownMenu',
  props: {
    cities: Array
  },
  data () {
    return {
      selected: 0
    }
  },
  methods: {
    selectCity () {
      // Send selected value to parent
      this.$emit('clicked', this.selected)
    }
  }
}
</script>

<style>
@import '../assets/main.css';
</style>
