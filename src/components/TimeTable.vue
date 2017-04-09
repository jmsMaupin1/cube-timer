<template>
  <div>
    <el-card class='time-table'>
      <el-table border 
        :data='tableData'
        max-height=520
        @row-click='showDialog'
        empty-text='Begin Solving!'>
        <el-table-column type='index' width=80 />
        <el-table-column prop='time' label='Time' />
      </el-table>

      <el-button v-on:click='clearData' class='btn-clear'>
        Clear Times
      </el-button>

      <div class="averages">
        <div class='ao5'>
          Average Of 5: <span class='average-time'>{{averageOf5}}</span>
        </div>

        <div class='ao12'>
          Average Of 12: <span class='average-time'>{{averageOf12}}</span>
        </div>
      </div>
    </el-card>

    <el-dialog
      title='Scramble Recap'
      v-model='dialogVisible'
    >
      <div>Scramble: {{scramble}}</div>      
      <CubeRep :scramble='scramble' />
      <div>Time: {{dialogTime}}</div>
    </el-dialog>
  </div>
</template>

<script>
import EventBus from './EventBus'
import CubeRep from './CubeRep'

export default {

  name: 'TimeTable',

  data () {
    return {
      tableData: [],
      dialogVisible: false,
      dialogTime: 0.00,
      scramble: ''
    }
  },
  components: {
    CubeRep
  },
  methods: {
    clearData: function () {
      this.tableData = []
    },
    showDialog: function (row, event, column) {
      this.scramble = row.scramble
      this.dialogTime = row.time
      this.dialogVisible = true
    }
  },
  created: function () {
    EventBus.$on('store', payload => {
      this.tableData.push(payload)
    })
  },
  computed: {
    // get the last 5 elements added to the array and then average them
    averageOf5: function () {
      let tableLength = this.tableData.length
      if (tableLength > 4) {
        return (this.tableData.slice(tableLength - 5)
          .reduce((total, next) => {
            return total + parseFloat(next.time)
          }, 0) / 5).toFixed(3)
      } else {
        return 'N/A'
      }
    },

    // same thing as above but the last 12 elements
    averageOf12: function () {
      let tableLength = this.tableData.length
      if (tableLength > 11) {
        return (this.tableData.slice(tableLength - 12)
          .reduce((total, next) => {
            return total + parseFloat(next.time)
          }, 0) / 12).toFixed(3)
      } else {
        return 'N/A'
      }
    }
  }
}
</script>

<style lang="css" scoped>
  .time-table {
    position: relative;
    float: left;
    width: 17%;
    height: 98vh;
  }

  .btn-clear {
    margin-top: 5px;
    width: 100%;
  }

  .ao5, .ao12 {
    padding-top: 20px;
  }

  .averages {
    width: 80%;
    position: absolute;
    bottom: 15px;
  }

  .average-time {
    float: right;
  }

  .scramble-dialog {
    background: gray;
  }
</style>
