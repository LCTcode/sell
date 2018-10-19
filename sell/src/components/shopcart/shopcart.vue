<template>
  <div class="shopcart">
    <div class="content" @click="toggle">
      <div class="content-left">
        <div class="logo-warp">
          <div class="num" v-show="totalCount>0">
            {{totalCount}}
          </div>
          <div class="logo" :class="{'lightHeight':totalCount>0}">
            <i class="icon-shopping_cart" :class="{'lightHeight':totalCount>0}"></i>
          </div>
        </div>
        <div class="price"  :class="{'lightHeight':totalPrice>0}">￥ {{totalPrice}}</div>
        <div class="desc">配送费 {{deliveryPrice}} ￥</div>
      </div>
      <div class="content-right">
        <div class="pay" :class="payClass">
          {{payDesc}}
        </div>
      </div>
    </div>
      <transition name="fold">
        <div class="shopcart-list" v-show="listShow">
          <div class="list-header">
            <h1 class="title">购物车</h1>
            <span class="empty" @click="empty">清空</span>
          </div>
          <div class="list-content" ref="listContent">
            <ul>
              <li class="food" v-for="(food,index) in selectFoods" :key="index">
                <span class="name">{{food.name}}</span>
                <div class="price">
                  <span>￥{{food.price*food.count}}</span>
                </div>
                <div class="cartcontrol-wrapper">
                  <control :food="food"></control>
                </div>
              </li>
            </ul>
          </div>
        </div>
      </transition>
  </div>
</template>
<script>
import control from '../../components/carcontrol/control'
import BScroll from 'better-scroll'
export default {
  props: {
    selectFoods: {
      type: Array,
      default () {
        return [
          {
            price: 10,
            count: 2
          }
        ]
      }
    },
    deliveryPrice: {
      type: Number,
      default: 0
    },
    minPrice: {
      type: Number,
      default: 0
    }
  },
  data () {
    return {
      fold: true
    }
  },
  computed: {
    totalPrice () {
      let total = 0
      this.selectFoods.forEach((food) => {
        total = food.price * food.count
      })
      return total
    },
    totalCount () {
      let count = 0
      this.selectFoods.forEach((food) => {
        count += food.count
      })
      return count
    },
    payDesc () {
      if (this.totalPrice === 0) {
        return `起送价￥ ${this.minPrice} 元`
      } else if (this.totalPrice < this.minPrice) {
        let diff = this.minPrice - this.totalPrice
        return `还差￥ ${diff} 元`
      } else {
        return `去结算`
      }
    },
    payClass () { // 控制样式，支付栏的钱够了背景绿
      if (this.totalPrice < this.minPrice) {
        return 'not-enough'
      } else {
        return 'enough'
      }
    },
    listShow () { // 购物车详情
      if (!this.totalCount) {
        return false
      }
      let show = !this.fold
      if (show) {
        this.$nextTick(() => {
          if (!this.scroll) {
            let _scroll = this.scroll
            _scroll = new BScroll(this.$refs.listContent, {
              click: true
            })
          } else {
            this.scroll.refresh()
          }
        })
      }
      return show
    }
  },
  components: {
    control
  },
  methods: {
    toggle () {
      if (!this.totalCount) {
        return
      }
      this.fold = !this.fold
    },
    empty () {
      this.selectFoods.forEach((food) => {
        food.count = 0
      })
    }
  }
}
</script>

<style scoped lang="stylus"  type="text/stylus">
  .shopcart
    position fixed
    left 0
    bottom 0
    width: 100%
    height 48px
    z-index: 50
    background #141d27
    .content
      border none
      display flex
      width: 100%
      background #141d27
      font-size 0
      .content-left
        flex 1
        .logo-warp
          position relative
          top: -10px
          display inline-block
          margin 0 12px
          padding:6px
          border-radius 50%
          background #141d27
          width:56px
          height:56px
          box-sizing border-box
          .num
            position absolute
            top:0
            left:0
            width:24px
            height:16px
            line-height 16px
            font-size 9px
            box-shadow 0 4px 8px 0 rgba(0,0,0,.4)
            font-weight:700
            background red
            color: #ffffff
            margin-left 28px
            border-radius 6px
            text-align center
          .logo
            width: 100%
            height: 100%
            background #2b343c
            border-radius 50%
            text-align center
            .icon-shopping_cart
              font-size 24px
              color #80858a
              line-height 44px // 56 - 12
              &.lightHeight
                color: #ffffff
            &.lightHeight
              color: #ffffff
              background rgb(0,160,200)
        .price
          display inline-block
          font-size 16px
          font-weight 700
          vertical-align top
          margin-top 12px
          line-height 24px
          box-sizing border-box
          padding-right 12px
          border-right 1px solid rgba(255,255,255,.1)
          color: rgba(255,255,255,.4)
          &.lightHeight
            color: #ffffff
        .desc
          display inline-block
          font-size 16px
          color: rgba(255,255,255,.4)
          font-weight 700
          line-height 24px
          margin 12px 0 0 12px
          vertical-align top
      .content-right
        flex 0 0 105px
        width:105px
        .pay
          text-align center
          font-size 16px
          color: rgba(255,255,255,.4)
          line-height 48px
          vertical-align: top
          &.not-enough
            background #2b343c
          &.enough
            background green
            color: #ffffff
    .shopcart-list
      position absolute
      left:0
      top:0
      width: 100%
      z-index: -1
      transform: translate3d(0, -100%, 0)
      &.fold-enter-active,&.fold-leave-active
        transition all 0.5s
      &.fold-enter,&.fold-leave-active
        transform translate3d(0,0,0)
      .list-header
        height:40px
        line-height:40px
        padding:0 18px
        background #f3f5f7
        border-bottom 1px solid rgba(7,17,27,.1)
        .title
          float: left
          font-size 14px
          color: rgb(7,17,27)
        .empty
          float: right
          font-size 12px
          color: rgb(0,160,220)

      .list-content
        padding:0 18px
        max-height 217px
        background #ffffff
        overflow hidden
        .food
          position relative
          padding:12px 0
          box-sizing border-box
          border 1px solid rgba(7,17,27,.1)
          .name
            line-height 24px
            font-size:14px
            color: rgb(7,17,27)
          .price
            position absolute
            right:90px
            bottom 12px
            line-height 12px
            font-size:14px
            font-weight 700
            color: red
          .cartcontrol-wrapper
            position absolute
            right 0
            bottom:6px
</style>
