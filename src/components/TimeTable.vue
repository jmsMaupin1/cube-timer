<template>
  <el-card class='time-table'>
    <el-table border 
      :data='tableData'
      max-height=520 
      empty-text=' '>
      <el-table-column type='index' width=80 />
      <el-table-column prop='time' label='Time' />
    </el-table>

    <div class='ao5'>
      Average Of 5: <span class='average'>{{averageOf5}}</span>
    </div>

    <div class='ao12'>
      Average Of 12: <span class='average'>{{averageOf12}}</span>
    </div>
  </el-card>
</template>

<script>
import EventBus from './EventBus'

export default {

  name: 'TimeTable',

  data () {
    return {
      tableData: []
    }
  },
  mounted: function () {
    EventBus.$on('store', payload => {
      this.tableData.push(payload)
    })
  },
  computed: {
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
    float: left;
    margin-top: 1%;
    width: 17%;
    height: 80vh;
  }

  .ao5, .ao12 {
    padding-top: 20px;
  }

  .average {
    float: right;
  }
</style>
