<template>
  <div class='timer'>
    <h1>{{time}}</h1>
  </div>
</template>

<script>
import EventBus from './EventBus'

export default {

  name: 'Timer',

  data () {
    return {
      nanosecs: 0,
      secs: 0,
      timer: false
    }
  },
  mounted: function () {
    EventBus.$on('space', () => {
      this.toggleClock()
    })
  },
  methods: {
    startClock: function () {
      this.nanosecs = 0
      this.secs = 0

      this.timer = setInterval(() => {
        this.nanosecs += 1
        if (this.nanosecs > 99) {
          this.nanosecs = 0
          this.secs += 1
        }
      }, 10)
    },
    stopClock: function () {
      clearInterval(this.timer)
      this.timer = false
      EventBus.$emit('clock-stopped', {time: this.time})
    },
    toggleClock: function () {
      if (this.timer) {
        this.stopClock()
      } else {
        this.startClock()
      }
    }
  },
  computed: {
    time: function () {
      return this.secs + '.' +
        (this.nanosecs < 10 ? '0' + this.nanosecs : this.nanosecs)
    }
  }
}
</script>

<style lang="css" scoped>
  .timer {
    line-height: 60vh;
    font-size: 4em;
    width: 80%;
    text-align: center;
  }
</style>
