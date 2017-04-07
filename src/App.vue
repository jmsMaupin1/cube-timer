<template>
  <div id="app">
    <scramble></scramble>
    <TimeTable></TimeTable>
    <timer></timer>
    <LiveCubeRep></LiveCubeRep>
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
