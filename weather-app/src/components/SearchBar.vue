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