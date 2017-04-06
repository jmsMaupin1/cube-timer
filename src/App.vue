<template>
  <div id="app">
    <scramble></scramble>
    <TimeTable></TimeTable>
    <timer></timer>
    <CubeRep scramble="R U R' U'"></CubeRep>
  </div>
</template>

<script>
import Scramble from './components/Scramble'
import Timer from './components/Timer'
import EventBus from './components/EventBus'
import TimeTable from './components/TimeTable'
import CubeRep from './components/CubeRepresentation'

export default {
  name: 'app',
  components: {
    Scramble,
    Timer,
    TimeTable,
    CubeRep
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
