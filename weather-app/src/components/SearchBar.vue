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
