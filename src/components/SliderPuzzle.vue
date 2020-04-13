<template>
  <div>
    <h1>{{ msg }}</h1>
    <div class="container">
      <h2 v-if="!puzzleCompleted"> Goodluck! </h2>
      <h2 v-else> Well done! :)</h2>
        <div class="blocks-frame">
          <Block v-for="block in blocks"
            :key="block.position"
            :block="block"
            @switch="moveBlock"
          />
        </div>
    </div>
  </div>
</template>

<script>

import Block from './Block'
import sample from 'lodash.sample'

export default {
  name: 'SliderPuzzle',
  components: { Block },
  props: {
    msg: String
  },
  data() {
    return {
      blockSize: {
        width: 125,
        height: 125,
      },
      size: {
        horizontal: 4,
        vertical: 4,
      },
      blocks: [],
    }
  },
  computed: {
    totalBlocks () {
      return this.size.horizontal * this.size.vertical;
    },
    puzzleCompleted () {
      if (!this.blocks.length) {
        return false;
      }
      for (let i = 0; i < this.totalBlocks; ++i) {
        if (this.blocks[i].styles.order !== this.blocks[i].position) {
          return false;
        }
      }
      return true;
    }
  },
  methods: {
    createPuzzleBlocks () {
      this.blocks = [];
      for (let i = 0; i < this.totalBlocks; ++i) {
        this.blocks.push({
          styles: {
            backgroundImage: i === 0 ? 'transparent' : 'url(' + require('../assets/images/digi.png') + ')',
            backgroundPositionX: `-${(i % this.size.horizontal) * this.blockSize.width}px`,
            backgroundPositionY: `-${Math.floor(i / this.size.horizontal) * this.blockSize.height}px`,
            width: `${this.blockSize.width}px`,
            height: `${this.blockSize.height}px`,
            order: i
          },
          position: i,
          isEmpty: i === 0,
        })
      }
    },
    moveBlock (block) {
      if (block.isEmpty) {
        return;
      }
      const target = this.blocks.find(b => {
        return b.isEmpty && this.nextPositionOrders(block).indexOf(b.styles.order) > -1;
      })
      target && this.changeBlocksOrder(target, block);
    },
    changeBlocksOrder (blockA, blockB) {
      [blockA.styles.order, blockB.styles.order] = [blockB.styles.order, blockA.styles.order];
    },
    nextPositionOrders (block) {
      const position = block.styles.order;
      return [
        position % this.size.horizontal ? position - 1 : null,
        (position + 1) % this.size.horizontal ? position + 1 : null,
        position - this.size.horizontal,
        position + this.size.horizontal
      ];
    },
    rearrangeBlocks () {
      for (let i = 0, j = this.totalBlocks * 5; i < j; ++i) {
        const emptyBlock = this.blocks.find(block => block.isEmpty);
        const movableBlocks = this.blocks.filter(block => {
          return this.nextPositionOrders(emptyBlock).indexOf(block.styles.order) > -1;
          });
        this.changeBlocksOrder(emptyBlock, sample(movableBlocks));
      }
    },
  },
  created() {
    // Game initialization. Create and scramble blocks.
    this.createPuzzleBlocks();
    this.rearrangeBlocks();
  }
}
</script>

<style lang="scss">
@import "src/assets/scss/sliderpuzzle.scss";
</style>

