<template>
  <div class="block" :style="block.styles" @click="switchBlock" :class="{ empty: block.isEmpty }" />
</template>

<script>
export default {
  props: ['block'],
  methods: {
    switchBlock () {
      this.$emit('switch', this.block)
    }
  }
}
</script>

<style lang="scss">
@import "src/assets/scss/block.scss";
</style>