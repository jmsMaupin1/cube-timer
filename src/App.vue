<template>
  <div id="app">
    <scramble></scramble>
  </div>
</template>

<script>
import Scramble from './components/Scramble'

export default {
  name: 'app',
  components: {
    Scramble
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
  margin-top: 60px;
}
</style>
