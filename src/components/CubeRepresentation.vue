<template>
    <el-card class="cube-rep">
        <canvas width='390' height='240' ref='canvas'></canvas>
    </el-card>
</template>

<script>
import EventBus from './EventBus'

export default {

  name: 'CubeRepresentation',
  props: ['scramble'],
  data () {
    return {
      // LURDFB
      cubeState: [
        0, 0, 0, 0, 0, 0, 0, 0, 0,
        1, 1, 1, 1, 1, 1, 1, 1, 1,
        2, 2, 2, 2, 2, 2, 2, 2, 2,
        3, 3, 3, 3, 3, 3, 3, 3, 3,
        4, 4, 4, 4, 4, 4, 4, 4, 4,
        5, 5, 5, 5, 5, 5, 5, 5, 5
      ]
    }
  },
  mounted: function () {
    EventBus.$on('space', () => {
      console.log('space')
      this.doSliceMove([
        15, 17, 11, 9,
        12, 16, 14, 10,
        8, 38, 18, 51,
        5, 37, 21, 52,
        2, 36, 24, 53
      ], "'")
    })
    this.drawCube()
  },
  directives: {},
  methods: {
    drawCube: function () {
      let ctx = this.$refs.canvas.getContext('2d')
      let cubieSize = 20
      let offset = 3 * cubieSize

      let colors = [
        'orange',
        'white',
        'red',
        'yellow',
        'green',
        'blue'
      ]

      // Each face consists of 9 cubies, the idea is to draw one face
      // and then change the offset to the right to draw another face
      // after draw the B and F faces above and below the U face respecitvely
      for (let face = 0; face < 4; ++face) {
        for (let cubie = 0; cubie < 9; ++cubie) {
          let index = (face * 9) + cubie

          let x = offset + (cubie % 3) * 20 + (offset * face)
          let y = offset * 1.5 + Math.floor(cubie / 3) * 20

          ctx.fillStyle = colors[this.cubeState[index]]
          ctx.fillRect(x, y, cubieSize - 2, cubieSize - 2)
        }
      }

      // Draw the F block (face 4)
      for (let cubie = 0; cubie < 9; ++cubie) {
        let index = (4 * 9) + cubie

        let x = offset * 2 + (cubie % 3) * 20
        let y = offset * 2.5 + Math.floor(cubie / 3) * 20

        ctx.fillStyle = colors[this.cubeState[index]]
        ctx.fillRect(x, y, cubieSize - 2, cubieSize - 2)
      }

      // Draw the B block (face 5)
      for (let cubie = 0; cubie < 9; ++cubie) {
        let index = 45 + cubie

        let x = offset * 2 + (cubie % 3) * 20
        let y = offset * 0.5 + Math.floor(cubie / 3) * 20

        ctx.fillStyle = colors[this.cubeState[index]]
        ctx.fillRect(x, y, cubieSize - 2, cubieSize - 2)
      }
    },
    doSliceMove: function (cycles, mod) {
      // take cycles as an array of 20 elements
      // decide how to cycle them with the mod
      switch (mod) {
        case "'":
          this.cycleStickers(
            cycles[3], cycles[2], 
            cycles[1], cycles[0])
          this.cycleStickers(
            cycles[7], cycles[6], 
            cycles[5], cycles[4])
          this.cycleStickers(
            cycles[11], cycles[10], 
            cycles[9], cycles[8])
          this.cycleStickers(
            cycles[15], cycles[14], 
            cycles[13], cycles[12])
          this.cycleStickers(
            cycles[19], cycles[18], 
            cycles[17], cycles[16])
          break
        case '2':
          break
        case '':
        default:
          break
      }

      this.drawCube()
    },
    doUMove: function (mod) {
      if (mod === '') {
        this.cycleStickers(15, 17, 11, 9)
        this.cycleStickers(12, 16, 14, 10)
        this.cycleStickers(8, 38, 18, 51)
        this.cycleStickers(5, 37, 21, 52)
        this.cycleStickers(2, 36, 24, 53)
      } else if (mod === "'") {
        this.cycleStickers(9, 11, 17, 15)
        this.cycleStickers(10, 14, 16, 12)
        this.cycleStickers(51, 18, 38, 8)
        this.cycleStickers(52, 21, 37, 5)
        this.cycleStickers(53, 24, 36, 2)
      } else if (mod === '2') {
        this.swapStickers(10, 16)
        this.swapStickers(12, 14)
        this.swapStickers(9, 17)
        this.swapStickers(11, 15)
        this.swapStickers(51, 38)
        this.swapStickers(52, 37)
        this.swapStickers(53, 36)
        this.swapStickers(2, 24)
        this.swapStickers(5, 21)
        this.swapStickers(8, 18)
      }

      this.drawCube()
    },
    cycleStickers: function (a, b, c, d) {
      let temp = this.cubeState[a]
      this.cubeState[a] = this.cubeState[b]
      this.cubeState[b] = this.cubeState[c]
      this.cubeState[c] = this.cubeState[d]
      this.cubeState[d] = temp
    },
    swapStickers: function (a, b) {
      let temp = this.cubeState[a]
      this.cubeState[a] = this.cubeState[b]
      this.cubeState[b] = temp
    }

  }
}
</script>

<style lang="css" scoped>
    .cube-rep {
        position: absolute;
        background: #ccc;
        width: 400px;
        height: 250px;
        bottom: 5px;
        right: 5px;
    }

    .cube-rep canvas {
      position: absolute;
      top: 5px;
      left: 5px;
    }
</style>
