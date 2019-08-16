<template>
  <div id="app">
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
  font-family:'Avenir', Helvetica, Arial, sans-serif;
  font-size: 20px;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
