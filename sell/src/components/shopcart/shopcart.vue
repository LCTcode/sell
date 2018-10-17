<template>
  <div class="shopcart">
    <div class="content">
      <div class="content-left">
        <div class="logo-warp">
          <div class="logo">
            <i class="icon-shopping_cart"></i>
          </div>
        </div>
        <div class="price">￥ 0</div>
        <div class="desc">配送费 {{deliveryPrice}} ￥</div>
      </div>
      <div class="content-right">
       ￥ {{minPrice}} 起送
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
        return []
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
        text-align center
        font-size 16px
        color: rgba(255,255,255,.4)
        background #2b343c
        line-height 48px
        vertical-align: top
</style>
