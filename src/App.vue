<template>
  <div id="app">
    <OpenWeatherMap msg="Welcome to Open Weather Map" />
  </div>
</template>

<script>
import OpenWeatherMap from './components/OpenWeatherMap.vue';

export default {
  name: 'App',
  components: {
    OpenWeatherMap
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
  margin-top: 60px;
}
</style>
