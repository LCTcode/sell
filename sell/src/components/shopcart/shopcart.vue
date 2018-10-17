<template>
  <div class="shopcart">
    <div class="content">
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
  </div>
</template>

<script>
export default {
  props: {
    selectFoods: {
      type: Array,
      default () {
        return [
          {
            price: 10,
            count: 1
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
      display flex
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
          line-height 48px
          padding-left 12px
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
</style>
