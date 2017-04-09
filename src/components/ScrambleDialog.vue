<template>
</template>

<script>
import CubeRep from './CubeRep'
// import EventBus from './EventBus'

export default {

  name: 'LiveCubeRep',
  props: ['scrambles', 'index'],
  components: {
    CubeRep
  },
  data () {
    return {
      currentScramble: -1
    }
  },
  watch: {
    scrambles: function (val) {
      console.log(val)
    },

    index: function (val) {
      console.log('index has changed ' + val)
    }
  }
}
</script>

<style lang="css" scoped>
</style>
