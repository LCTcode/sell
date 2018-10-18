<template>
   <div class="control-wrap">
     <transition name="move">
     <div class="decrease" @click="decreaseCart" v-show="food.count>0">
       <span class="inner icon-remove_circle_outline"></span>
     </div>
     </transition>
     <div class="cart-count" v-show="food.count>0">{{food.count}}</div>
     <div class="increase icon-add_circle" @click="addCart"></div>
   </div>
</template>

<script>
import Vue from 'vue'
export default {
  props: {
    food: {
      type: Object
    }
  },
  created () {
    // console.log(this.food)
  },
  methods: {
    addCart (event) {
      if (!event._constructed) {
        return
      }
      if (!this.food.count) {
        Vue.set(this.food, 'count', 1)
      } else {
        this.food.count++
      }
    },
    decreaseCart (event) {
      if (!event._constructed) {
        return
      }
      if (!this.food.count) {
        Vue.set(this.food, 'count', 1)
      } else {
        this.food.count--
      }
    }
  }
}
</script>

<style scoped lang="stylus"  type="text/stylus">

  .control-wrap
    font-size 0
    .move-enter-active,.move-leave-active
      transition all .5s linear
      opacity 1
      transform translate3d(24,0,0)
      .inner
        display inline-block
        transform rotate(0deg)
        transition all .5s linear
    .move-enter,.move-leave-active
      opacity 0
      transform translate3d(0,0,0)
      .inner
        display inline-block
        transform rotate(180deg)
        transition all .5s linear
    .decrease
      display inline-block
      display inline-block
      line-height 24px
      font-size 24px
      margin-left 5px
      .icon-remove_circle_outline
        color: rgb(0,160,220)
    .cart-count
      display inline-block
      font-size 10px
      width: 24px
      text-align center
      vertical-align top
      line-height 24px
      padding-top:6
      vertical-align top
      color: rgb(147,153,159)
    .increase
      display inline-block
      line-height 24px
      font-size 24px
    .icon-add_circle
      color: rgb(0,160,220)
</style>
