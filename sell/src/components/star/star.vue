<template>
  <div class="star" :class="starType">
    <span v-for="itemClass in itemClasses" :class="itemClass" class="star-item" :key="itemClass.id"></span>
  </div>
</template>
<script>
const LENGTH = 5
const CLS_ON = 'on'
const CLS_HALF = 'half'
const CLS_OFF = 'off'
export default {
  props: {
    size: {
      type: Number
    },
    score: {
      type: Number,
      default: 5
    }
  },
  computed: {
    starType () {
      return 'star-' + this.size
    },
    itemClasses () {
      let result = []
      let score = Math.floor(this.score * 2) / 2
      console.log(score + '----1')
      let hasDecimal = score % 1 !== 0 // score有小数，则添加半颗星星
      let integer = Math.floor(score) // 整数个星星
      console.log(score + '----2')

      for (let i = 0; i < integer; i++) {
        result.push(CLS_ON)
        console.log('a---')
      }
      if (hasDecimal) {
        result.push(CLS_HALF)
        console.log('b===')
      }
      while (result.length < LENGTH) { // 将不足的地方添加0颗星星
        result.push(CLS_OFF)
        console.log('c+++')
      }
      return result
    }
  }
}
</script>

<style scoped lang="stylus" type="text/stylus">
  @import "../../common/stylus/mixin.styl"
  .star
    font-size 0
    .star-item
      display inline-block
      background-repeat no-repeat
    &.star-48
      .star-item
        width:20px
        height:20px
        background-size 20px 20px
        margin-right 22px
        &:last-child
          margin-right 0
        &.on
          bg-image('star48_on')
        &.half
          bg-image('star48_half')
        &.ff
          bg-image('star48_off')
    &.star-48
      .star-item
        width:20px
        height:20px
        background-size 20px 20px
        margin-right 22px
        &:last-child
          margin-right 0
        &.on
          bg-image('star48_on')
        &.half
          bg-image('star48_half')
        &.ff
          bg-image('star48_off')
    &.star-36
      .star-item
        width:15px
        height:15px
        background-size 15px 15px
        margin-right 6px
        &:last-child
          margin-right 0
        &.on
          bg-image('star36_on')
        &.half
          bg-image('star36_half')
        &.ff
          bg-image('star36_off')
    &.star-24
      .star-item
        width:10px
        height:10px
        background-size 10px 10px
        margin-right 6px
        &:last-child
          margin-right 0
        &.on
          bg-image('star24_on')
        &.half
          bg-image('star24_half')
        &.ff
          bg-image('star24_off')
</style>
