<template>
  <div id="app">
    <h1>Sniper - Command Generator</h1>
    <Sniper/>
  </div>
</template>

<script>
import Sniper from './components/Sniper.vue'

export default {
  name: 'app',
  components: {
    Sniper
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
