<template>
  <div id="app">
    <LiveCubeRep></LiveCubeRep>
    <TimeTable></TimeTable>
    <scramble></scramble>
    <timer></timer>
  </div>
</template>

<script>
import Scramble from './components/Scramble'
import Timer from './components/Timer'
import EventBus from './components/EventBus'
import TimeTable from './components/TimeTable'
import LiveCubeRep from './components/LiveCubeRep'

export default {
  name: 'app',
  components: {
    Scramble,
    Timer,
    TimeTable,
    LiveCubeRep
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
}
</style>
