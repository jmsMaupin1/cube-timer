<template>
  <div id="app">
    <scramble></scramble>
    <timer></timer>
  </div>
</template>

<script>
import Scramble from './components/Scramble'
import Timer from './components/Timer'
import EventBus from './components/EventBus'

export default {
  name: 'app',
  components: {
    Scramble,
    Timer
  },
  created: function () {
    window.addEventListener('keyup', this.keyup)
  },
  methods: {
    keyup: function (e) {
      if (e.code === 'Space') {
        EventBus.$emit('space')
      }
    }
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
