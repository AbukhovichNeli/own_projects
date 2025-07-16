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