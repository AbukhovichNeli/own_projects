<template>
  <div id="app">
    <h1>Weather Forecast</h1>
    <SearchBar @location-selected="onLocationSelected" />
    <WeatherDisplay v-if="selectedLocation" :location="selectedLocation" />
  </div>
</template>

<script>
import SearchBar from "./components/SearchBar.vue";
import WeatherDisplay from "./components/WeatherDisplay.vue";

export default {
  components: {
    SearchBar,
    WeatherDisplay,
  },
  data() {
    return {
      selectedLocation: null,
    };
  },
  methods: {
    onLocationSelected(location) {
      this.selectedLocation = location;
    },
  },
};
</script>
