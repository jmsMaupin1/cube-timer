<template>
  <div class="scramble">
    <el-card class="box-card">
      <h1>
        {{scramble}}
      </h1>
    </el-card>
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
  beforeCreate: function () {
    EventBus.$emit('test')
  },
  created: function () {
    this.getScramble()
  },
  mounted: function () {
    EventBus.$on('clock-stopped', payload => {
      EventBus.$emit('store', {
        time: payload.time,
        scramble: this.scramble
      })
      this.getScramble()
    })
  },
  methods: {
    getScramble: function () {
      /*
        There are three axis' in general x, y, and z
        On a cube any axis has two slices associated with it
        for instance the x axis corresponds with the Right and Left
        slices.

        We want to make sure that any move is not preceeded or followed
        by a move on the same axis. Because if you move any two slices on
        the same axis they do not effect each other creating potential for not only redundant moves, but also for moves that do not actually scramble the cube
      */
      let axis = [['U', 'D'], ['F', 'B'], ['R', 'L']]
      // you can either turn a slice:
      // clockwise ' ', counterclockwise '\'', 180 degrees '2'
      let mods = ['', '\'', '2']
      let lastAxis = -1
      let currentAxis = -1

      this.scramble = ''
      for (var i = 0; i < 25; ++i) {
        // make sure the next move we make is from a different axis than the last move we made
        do {
          currentAxis = Math.floor(Math.random() * 3)
        } while (currentAxis === lastAxis)

        let move = Math.floor(Math.random() * 2)
        let mod = Math.floor(Math.random() * 3)

        this.scramble += axis[currentAxis][move] + mods[mod] + ' '

        lastAxis = currentAxis
      }
      EventBus.$emit('scramble-generated', this.scramble.trim())
    }
  }
}
</script>

<style lang="css" scoped>
  .box-card {
    text-align: center;
  }
</style>
