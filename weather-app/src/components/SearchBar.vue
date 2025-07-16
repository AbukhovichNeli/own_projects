<template>
  <div>
    <input v-model="query" @keyup.enter="searchLocation" placeholder="Enter city name" />
    <ul v-if="results.length">
      <li v-for="loc in results" :key="loc.id" @click="selectLocation(loc)">
        {{ loc.name }}, {{ loc.country }}
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return {
      query: "",
      results: [],
    };
  },
  methods: {
    async searchLocation() {
      try {
        const res = await fetch(
          `https://geocoding-api.open-meteo.com/v1/search?name=${this.query}&count=5&language=en&format=json`
        );
        const data = await res.json();
        this.results = data.results || [];
      } catch (error) {
        console.error("Error fetching locations:", error);
      }
    },
    selectLocation(location) {
      this.$emit("location-selected", location);
      this.results = [];
      this.query = "";
    },
  },
};
</script>

<style scoped>
li {
  list-style: none;
  cursor: pointer;
}

input {
  width: 100%;
  max-width: 400px;
  padding: 0.5rem 0.75rem;
  font-size: 1rem;
  border: 1px solid #ccc;
  border-radius: 6px;
  outline-offset: 2px;
  transition: border-color 0.3s ease, box-shadow 0.3s ease;
}
</style>