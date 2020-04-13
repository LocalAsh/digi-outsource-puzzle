<template>
  <div id="app">
    <SliderPuzzle msg="DigiOutsource Puzzle"/>
  </div>
</template>

<script>
import SliderPuzzle from './components/SliderPuzzle.vue'

export default {
  name: 'App',
  components: {
    SliderPuzzle
  }
}
</script>

<style lang="scss">
@import "src/assets/scss/styles.scss";
</style>
