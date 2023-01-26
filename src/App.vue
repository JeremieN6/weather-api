<template>
  <WeatherApp />
</template>

<script>
import WeatherApp from './components/WeatherApp.vue'

export default {
  name: 'App',
  components: {
    WeatherApp
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
