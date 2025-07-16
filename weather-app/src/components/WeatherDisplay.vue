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

<style scoped>
.scroll-container {
  display: flex;
  overflow-x: auto;
  padding: 1rem 0;
  gap: 1rem;
  scrollbar-width: thin;
}

.hour-circle {
  flex: 0 0 auto;
  width: 100px;
  height: 100px;
  border-radius: 30%;
  background: linear-gradient(135deg, #4f46e5, #3b82f6);
  color: white;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  font-weight: 600;
  box-shadow: 0 0 10px rgba(59, 130, 246, 0.6);
  user-select: none;
  cursor: default;
}

.time {
  font-size: 0.9rem;
  margin-bottom: 0.2rem;
}

.temp {
  font-size: 1.2rem;
}
</style>
