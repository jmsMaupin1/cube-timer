<template>
  <div class="scramble">
    <h1>
      {{scramble}}
    </h1>
  </div>
</template>

<script>
import EventBus from './EventBus'

export default {

  name: 'Scramble',

  data () {
    return {
      scramble: ''
    }
  },
  created: function () {
    this.getScramble()
  },
  mounted: function () {
    EventBus.$on('clock-stopped', () => {
      this.getScramble()
    })
  },
  methods: {
    getScramble: function () {
      let axis = [['U', 'D'], ['F', 'B'], ['R', 'L']]
      let mods = [' ', '\'', '2']
      let lastAxis = -1
      let currentAxis = -1

      this.scramble = ''
      for (var i = 0; i < 25; ++i) {
        do {
          currentAxis = Math.floor(Math.random() * 3)
        } while (currentAxis === lastAxis)

        let move = Math.floor(Math.random() * 2)
        let mod = Math.floor(Math.random() * 3)

        this.scramble += axis[currentAxis][move] + mods[mod] + ' '

        lastAxis = currentAxis
      }
    }
  }
}
</script>

<style lang="css" scoped>
</style>
