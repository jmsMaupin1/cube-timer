<template>
  <el-card class='time-table'>
    <el-table border :data='tableData' style='width: 100%'>
      <el-table-column type='index'>
      </el-table-column>
      <el-table-column prop='time' label='Time'>
      </el-table-column>
    </el-table>
  </el-card>
</template>

<script>
import EventBus from './EventBus'

export default {

  name: 'TimeTable',

  data () {
    return {
      tableData: [{}]
    }
  },
  mounted: function () {
    EventBus.$on('store', payload => {
      this.tableData.push(payload)
    })
  }
}
</script>

<style lang="css" scoped>
  .time-table {
    float: left;
    margin-top: 1%;
    width: 15%;
    height: 80vh;
  }
</style>
