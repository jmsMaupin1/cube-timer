<template>
  <canvas width='390' height='240' ref='canvas'></canvas>
</template>

<script>
export default {

  name: 'CubeRep',
  props: ['scramble'],
  data () {
    return {
      // Each number corresponds to a sticker color
      // Orange, White, Red, Yellow, Green, Blue
      initialCubeState: [
        0, 0, 0, 0, 0, 0, 0, 0, 0,
        1, 1, 1, 1, 1, 1, 1, 1, 1,
        2, 2, 2, 2, 2, 2, 2, 2, 2,
        3, 3, 3, 3, 3, 3, 3, 3, 3,
        4, 4, 4, 4, 4, 4, 4, 4, 4,
        5, 5, 5, 5, 5, 5, 5, 5, 5
      ],

      // an array container arrays of stickers to cycle or swap
      sliceMoves: [
        // Cycle array for L moves
        [
          [5, 1, 3, 7],
          [8, 2, 0, 6],
          [45, 35, 36, 9],
          [48, 32, 39, 12],
          [51, 29, 42, 15]
        ],
        // Cycle array for U moves
        [
          [12, 16, 14, 10],
          [15, 17, 11, 9],
          [8, 38, 18, 51],
          [5, 37, 21, 52],
          [2, 36, 24, 53]
        ],
        // Cycle array for R moves
        [
          [25, 23, 19, 21],
          [26, 20, 18, 24],
          [38, 33, 47, 11],
          [41, 30, 50, 14],
          [44, 27, 53, 17]
        ],
        // Cycle array for D movs
        [
          [30, 34, 32, 28],
          [33, 35, 29, 27],
          [47, 26, 42, 0],
          [46, 23, 43, 3],
          [45, 20, 44, 6]
        ],
        // Cycle array for F moves
        [
          [43, 41, 37, 39],
          [42, 44, 38, 36],
          [6, 33, 24, 15],
          [7, 34, 25, 16],
          [8, 35, 26, 17]
        ],
        // Cycle array for B moves
        [
          [52, 50, 46, 48],
          [53, 47, 45, 51],
          [0, 9, 18, 27],
          [1, 10, 19, 28],
          [2, 11, 20, 29]
        ]
      ]
    }
  },
  mounted: function () {
    this.drawCube()
  },
  watch: {
    cubeState: function (val) {
      this.drawCube()
    }
  },
  computed: {
    // arrays are pass-by-ref so we first make a copy of the initialCubeState so that we dont modify it in doScramble. This is particularly important so whenever a new scramble is generated, it doesnt just apply it to the cubeState based on the last scramble
    cubeState: function () {
      let cubeData = this.initialCubeState.slice(0)

      if (this.scramble) {
        this.doScramble(cubeData, this.scramble.trim())
      }

      return cubeData
    }
  },
  methods: {
    // because arrays are pass-by-ref we dont actually need to return anything
    cycleStickers: function (cubeState, a, b, c, d) {
      let temp = cubeState[a]
      cubeState[a] = cubeState[b]
      cubeState[b] = cubeState[c]
      cubeState[c] = cubeState[d]
      cubeState[d] = temp
    },
    swapStickers: function (cubeState, a, b) {
      let temp = cubeState[a]
      cubeState[a] = cubeState[b]
      cubeState[b] = temp
    },
    // This takes the arrays from this.sliceMoves and cycles or swaps the stickers around depending on the modifier. 2 is 180 degree rotation while prime("'") and no modifier are 90 degree rotations counter clockwise and clockwise respectively
    doSlice: function (cubeState, swapArrays, mod) {
      switch (mod) {
        case '2':
          for (let arrays in swapArrays) {
            this.swapStickers(cubeState,
              swapArrays[arrays][0], swapArrays[arrays][2])

            this.swapStickers(cubeState,
              swapArrays[arrays][1], swapArrays[arrays][3])
          }
          break
        case "'":
          for (let i = 0; i < swapArrays.length; ++i) {
            this.cycleStickers(cubeState,
              swapArrays[i][3],
              swapArrays[i][2],
              swapArrays[i][1],
              swapArrays[i][0])
          }
          break
        case undefined:
          for (let i = 0; i < swapArrays.length; ++i) {
            this.cycleStickers(cubeState,
              swapArrays[i][0],
              swapArrays[i][1],
              swapArrays[i][2],
              swapArrays[i][3])
          }
          break
        default:
          break
      }
    },
    // Parses this.scramble and performs the correct slice moves with the correct modifier
    doScramble: function (cubeState, scramble) {
      let moves = scramble.split(' ')
      let possibleMoves = 'LURDFB'
      for (let index in moves) {
        let move = moves[index]
        let slice = possibleMoves.indexOf(move.split('')[0])
        let mod = move.split('')[1]
        this.doSlice(cubeState, this.sliceMoves[slice], mod)
      }
    },
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
        let bgX = (offset + (offset + 5) * face) - 2
        let bgY = (offset * 1.5) - 2
        let size = offset + 2

        ctx.fillStyle = 'black'
        ctx.fillRect(bgX, bgY, size, size)
        for (let cubie = 0; cubie < 9; ++cubie) {
          let index = (face * 9) + cubie

          let x = offset + (cubie % 3) * 20 + ((offset + 5) * face)
          let y = offset * 1.5 + Math.floor(cubie / 3) * 20

          // ctx.fillStyle = 'black'
          // ctx.fillRect(x - 2, y - 2, cubieSize + 2, cubieSize + 2)

          ctx.fillStyle = colors[this.cubeState[index]]
          ctx.fillRect(x, y, cubieSize - 2, cubieSize - 2)
        }
      }

      // Draw the F block (face 4)
      for (let cubie = 0; cubie < 9; ++cubie) {
        let index = (4 * 9) + cubie

        let x = (offset + 2.5) * 2 + (cubie % 3) * 20
        let y = (offset + 2.5) * 2.5 + Math.floor(cubie / 3) * 20

        ctx.fillStyle = 'black'
        ctx.fillRect(x - 2, y - 2, cubieSize + 2, cubieSize + 2)

        ctx.fillStyle = colors[this.cubeState[index]]
        ctx.fillRect(x, y, cubieSize - 2, cubieSize - 2)
      }

      // Draw the B block (face 5)
      for (let cubie = 0; cubie < 9; ++cubie) {
        let index = 45 + cubie

        let x = (offset + 2.5) * 2 + (cubie % 3) * 20
        let y = (offset - 10) * 0.5 + Math.floor(cubie / 3) * 20

        ctx.fillStyle = 'black'
        ctx.fillRect(x - 2, y - 2, cubieSize + 2, cubieSize + 2)

        ctx.fillStyle = colors[this.cubeState[index]]
        ctx.fillRect(x, y, cubieSize - 2, cubieSize - 2)
      }
    }
  }
}
</script>

<style lang="css" scoped>
</style>
