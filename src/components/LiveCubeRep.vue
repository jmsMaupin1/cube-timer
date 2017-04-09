<!-- This component is just a wrapper component to position the CubeRep in the bottom Right and update the scramble as its generated -->
<template>
  <el-card class='live-cube-rep'>
    <CubeRep v-bind:scramble='scramble'></CubeRep>
  </el-card>
</template>

<script>
import CubeRep from './CubeRep'
import EventBus from './EventBus'

export default {

  name: 'LiveCubeRep',
  components: {
    CubeRep
  },

  data () {
    return {
      scramble: ''
    }
  },
  created: function () {
    EventBus.$on('scramble-generated', payload => {
      this.scramble = payload
    })
  }
}
</script>

<style lang="css" scoped>
  .live-cube-rep {
        position: absolute;
        background: #ccc;
        width: 400px;
        height: 250px;
        bottom: 5px;
        right: 5px;
    }
</style>
