<template>
  <canvas width='390' height='240' ref='canvas'></canvas>
</template>

<script>

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
      ],

      sliceMoves: [
        // Cycle array for L moves
        [7, 3, 1, 5, 8, 2, 0, 6, 45, 35, 36,
          9, 48, 32, 39, 12, 51, 29, 42, 15],
        // Cycle array for U moves
        [12, 16, 14, 10, 15, 17, 11, 9, 8, 38,
          18, 51, 5, 37, 21, 52, 2, 36, 24, 53],
        // Cycle array for R moves
        [25, 23, 19, 21, 26, 20, 18, 24, 38, 33,
          47, 11, 41, 30, 50, 14, 44, 27, 53, 17],
        // Cycle array for D movs
        [30, 34, 32, 28, 33, 35, 29, 27, 47, 26,
          42, 0, 46, 23, 43, 3, 45, 20, 44, 6],
        // Cycle array for F moves
        [43, 41, 37, 39, 42, 44, 38, 36, 6, 33,
          24, 15, 7, 34, 25, 16, 8, 35, 26, 17],
        // Cycle array for B moves
        [48, 46, 50, 52, 51, 45, 47, 53, 0, 9,
          18, 27, 1, 10, 19, 28, 2, 11, 20, 29]
      ]
    }
  },
  mounted: function () {
    if (this.scramble) {
      this.doScramble(this.scramble)
    }
    this.drawCube()
  },
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
    },
    doSlice: function (cycles, mod) {
      // take cycles as an array of 20 elements
      // decide in which order to cycle/swap based on the moves modifier
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
          this.swapStickers(cycles[0], cycles[2])
          this.swapStickers(cycles[1], cycles[3])
          this.swapStickers(cycles[4], cycles[6])
          this.swapStickers(cycles[5], cycles[7])
          this.swapStickers(cycles[8], cycles[10])
          this.swapStickers(cycles[9], cycles[11])
          this.swapStickers(cycles[12], cycles[14])
          this.swapStickers(cycles[13], cycles[15])
          this.swapStickers(cycles[16], cycles[18])
          this.swapStickers(cycles[17], cycles[19])
          break
        case '':
        default:
          this.cycleStickers(
            cycles[0], cycles[1],
            cycles[2], cycles[3])
          this.cycleStickers(
            cycles[4], cycles[5],
            cycles[6], cycles[7])
          this.cycleStickers(
            cycles[8], cycles[9],
            cycles[10], cycles[11])
          this.cycleStickers(
            cycles[12], cycles[13],
            cycles[14], cycles[15])
          this.cycleStickers(
            cycles[16], cycles[17],
            cycles[18], cycles[19])
          break
      }
    },
    doScramble: function () {
      let scrambleArray = this.scramble.split(' ')
      let possibleMoves = 'LURDFB'

      for (let index in scrambleArray) {
        let move = scrambleArray[index]
        let slice = move.split('')[0]
        let mod = move.split('')[1]

        this.doSlice(this.sliceMoves[possibleMoves.indexOf(slice)], mod)
      }
      this.drawCube()
    }
  }
}
</script>

<style lang="css" scoped>
    .cube-rep canvas {
      position: absolute;
      top: 5px;
      left: 5px;
    }
</style>
<!-- 
   B
//LURD
   F
         45 46 47
         48 49 50
         51 52 53

  0 1 2   9 10 11  18 19 20  27 28 29
  3 4 5  12 13 14  21 22 23  30 31 32
  6 7 8  15 16 17  24 25 26  33 34 35

         36 37 38
         39 40 41
         42 43 44

L:  7, 3, 1, 5,
    8, 2, 0, 6,
    45, 35, 36, 9,
    48, 32, 39, 12,
    51, 29, 42, 15  

R:  25, 23, 19, 21,
    26, 20, 18, 24,
    38, 33, 47, 11,
    41, 30, 50, 14,
    44, 27, 53, 17

B:  48, 46, 50, 52,
    51, 45, 47, 53,
    0, 9, 18, 27,
    1, 10, 19, 28,
    2, 11, 20, 29

F:  43, 41, 37, 39,
    42, 44, 38, 36,
    6, 33, 24, 15, 
    7, 34, 25, 16,
    8, 35, 26, 17

U:  12, 16, 14, 10,
    15, 17, 11, 9,
    8, 38, 18, 51,
    5, 37, 21, 52,
    2, 36, 24, 53

D:  30, 34, 32, 28, 
    33, 35, 29, 27, 
    47, 26, 42, 0,
    46, 23, 43, 3,
    45, 20, 44, 6
 -->
