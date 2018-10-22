<template>
  <transition name="showFood">
   <div class="food" v-show="showFlag" ref="foodsScroll">
    <div class="food-content">
      <div class="image-header">
        <img :src="food.image" alt="">
        <div class="back" @click="hide">
          <i class="icon-arrow_lift"></i>
        </div>
      </div>
      <div class="content">
        <h1 class="title">{{food.name}}</h1>
        <div class="food-detail">
          <span class="sell-count">月售:{{food.sellCount}}</span>
          <span class="rating">好评率:{{food.rating}}</span>
        </div>
        <div class="price">
          <span class="now">￥{{food.price}}</span><!--
          --><span class="old" v-show="food.oldPrice">￥{{food.oldPrice}}</span>
        </div>
        <div class="cartcontrol-wrap">
          <control :food="food"></control>
        </div>
        <div @click="addFirst($event)" class="buy" v-show="!food.count || food.count==0">
          加入购物车
        </div>
        </div>
      <split></split>
      <div class="info" v-show="food.info">
        <h1 class="title" v-show="food.info">商品信息</h1>
        <p class="text">
          {{food.info}}
        </p>
      </div>
      <split></split>
      <div class="rating">
        <h1 class="title">商品信息</h1>
      </div>
      </div>
    </div>
  </transition>
</template>

<script>
import BScroll from 'better-scroll'
import Vue from 'vue'
import control from '../../components/carcontrol/control'
import split from '../../components/split/split'
export default {
   props: {
     food: {
       type: Object
     }
   },
  data () {
    return {
      showFlag: false
    }
  },
  methods: {
     show () {
       this.showFlag = true
       this.$nextTick(() => {
         if (!this.scroll) {
           this.scroll = new BScroll(this.$refs.foodsScroll, {
             click: true
           })
         }else {
           this.scroll.refresh()
         }
       })
     },
    hide () {
      this.showFlag = false
    },
    addFirst (event) {
       if(!event._constructed) {
         return
       }
       Vue.set(this.food, 'count', 1)
    }
  },
  components: {
    control,
    split
  }
}
</script>

<style scoped lang="stylus"  type="text/stylus">
@import '../../common/stylus/style.styl'
  .food
    position fixed
    width: 100%
    top:0
    left:0
    bottom:48px
    z-index: 30
    background #ffffff
    transition all .5s
&.showFood-enter-active,&.showFood-leave-active
    transform translate3d(-100%, 0, 0)
  &.showFood-enter,&.showFood-leave-active
    transform translate3d(100%, 0, 0)
  .image-header
    position relative
    width: 100%
    height:0
    padding-bottom 100%
    img
      position absolute
      left 0
      top 0
      width: 100%
      height: 100%
  .back
    position absolute
    top:10px
    left:0
    color: #ffffff
    .icon-arrow_lift
      display block
      padding:10px
      font-size 20px
  .content
    position relative
    padding:18px
    width:100%
    .title
      line-height 14px
      margin-bottom 8px
      font-size 14px
      font-width: 700
      color: rgb(7,17,27)
    .food-detail
      margin-bottom 18px
      line-height 10px
      height:10px
      .sell-count
        font-size 10px
        color: rgb(147,153,159)
        margin-right: 12px
    .price
      font-weight 700
      line-height 24px
      .now
        margin-right 8px
        font-size 14px
        color: red
      .old
        text-decoration line-through
        font-size 10px
        color rgb(147,153,159)
    .cartcontrol-wrap
      position absolute
      right:80px
      bottom 18px
    .buy
      position absolute
      right:80px
      bottom 18px
      z-index 10
      line-height 24px
      height:24px
      padding:0 12px
      font-size 10px
      box-sizing border-box
      border-radius 12px
      color: #ffffff
      background rgb(0,160,220)
  .info
    padding:18px
    .title
      line-height 14px
      margin-bottom 16px
      font-size 14px
      color: rgb(7,17,27)
    .text
      line-height: 24px
      padding:0 8px
      font-size 12px
      color: rgb(77,68,93)
</style>
