<template>
  <div v-if="weather">
    <h2>Weather in {{ location.name }}</h2>
    <div class="scroll-container">
      <div v-for="(temp, i) in weather.hourly.temperature_2m.slice(0, 24)" :key="i" class="hour-circle">
        <div class="time">{{ formatHour(weather.hourly.time[i]) }}</div>
        <div class="temp">{{ temp }}°C</div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: ['location'],
  data() {
    return { weather: null }
  },
  watch: {
    location: 'fetchWeather'
  },
  methods: {
    async fetchWeather() {
      if (!this.location) return
      const { latitude, longitude } = this.location
      const today = new Date().toISOString().split('T')[0]
      const tomorrow = new Date(Date.now() + 86400000).toISOString().split('T')[0]

      const res = await fetch(
        `https://api.open-meteo.com/v1/forecast?latitude=${latitude}&longitude=${longitude}&hourly=temperature_2m&start_date=${today}&end_date=${tomorrow}&timezone=auto`
      )
      const data = await res.json()
      this.weather = data
    },
    formatHour(dateStr) {
      return new Date(dateStr).toLocaleTimeString([], { hour: '2-digit', minute: '2-digit' })
    }
  }
}
</script>
