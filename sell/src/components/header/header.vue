<template>
  <div class="header">
    <div class="content-wrapper">
      <div class="avatar">
        <img :src="seller.avatar" alt="seller-avatar" width="64" height="64">
      </div><!--
      --><div class="content"> <!--注释去除这里的图片与文字空隙，还可以用font-size 0解决-->
        <div class="title">
          <span class="brand"></span>
          <span class="name">{{seller.name}}</span>
        </div>
        <div class="description">
          <span>{{seller.description}}/{{seller.deliveryTime}} 分钟送达</span>
        </div>
        <div class="supports" v-if="seller.supports"> <!--如果不判断则会报错，因为一开始seller初始化为kong-->
          <span class="icon" :class="classMap[seller.supports[0].type]"></span>
          <span class="text">{{seller.supports[0].description}}</span>
        </div>
      </div>
      <div class="supports-count" v-if="seller.supports">
        <div class="count">{{seller.supports.length}}个</div>
        <i class="icon-keyboard_arrow_right"></i>
      </div>
    </div>
    <div class="bulletin-wrapper">
      <span class="bulletin-title"></span><!--
      --><span class="bulletin-text">{{seller.bulletin}}</span>
      <i class="icon-keyboard_arrow_right"></i>
    </div>
    <div class="backgroun">
      <img :src="seller.avatar" alt="" width="100%" height="100%">
    </div>
  </div>
</template>

<script>
export default {
  props: {
    seller: {
      type: Object
    }
  },
  created () {
    this.classMap = ['decrease', 'discount', 'guarantee', 'invoice', 'special'] // 巧妙 classMap[seller.supports[0].type]
  }
}
</script>

<style lang="stylus"  type="text/stylus">
  @import "../../common/stylus/mixin.styl"
  @import "../../common/stylus/base.styl"
  @import "../../common/stylus/icon.styl"

  .header
    position relative
    color: #fff
    background rgba(7,17,27,.5)
  .content-wrapper
    padding: 24px 12px 18px 24px;
    position relative
  .avatar
    display: inline-block
    vertical-align :top
    img
      display inline-block
      border-radius 2px
  .content
    position relative
    display: inline-block
    font-size: 14px
    margin-left: 16px
    .title
        margin: 2px 0 8px 0
        .brand
          display: inline-block
          width: 30px
          height: 18px
          bg-image("brand")
          background-size :30px 18px
          background-repeat :no-repeat
          vertical-align :top
        .name
          margin-left:6px
          font-size: 16px
          line-height: 18px
          font-weight: bold
    .description
      margin-bottom 10px
      line-height 12px
      font-size 12px
    .supports
      .icon
        display inline-block
        width:12px
        height:12px
        margin-right 4px
        background-size 12px
        background-repeat no-repeat
        vertical-align top
        &.decrease
          bg-image("decrease_1")
        &.discount
          bg-image("discount_1")
        &.guarantee
          bg-image("guarantee_1")
        &.invoice
          bg-image("invoice_1")
        &.special
          bg-image("special_1")
      .text
        line-height 12px
        font-size 12px
        vertical-align top
  .supports-count
    position absolute
    right:12px
    padding:0 14px
    height 24px
    line-height 24px
    border-radius 14px
    background-color rgba(0,0,0,.2)
    text-align center
    top:67px
    .count
      vertical-align top
      font-size 10px
    .icon-keyboard_arrow_right
      position absolute
      text-align center
      font-size 10px
      bottom 7px
      right:3px
      margin-left 2px
  .bulletin-wrapper
    position relative
    height:28px
    line-height 28px
    padding:0 22px 0 12px
    white-space nowrap
    overflow: hidden
    text-overflow ellipsis
    background rgba(7,17,27,.2)
    .bulletin-title
      display inline-block
      width:22px
      height:12px
      bg-image("bulletin")
      background-size 22px 12px
      background-repeat no-repeat
    .bulletin-text
      font-size 10px
      font-weight 200
      vertical-align top
      margin 0 4px
    .icon-keyboard_arrow_right
      position absolute
      font-size 10
      right:8px
      top:6px
  .backgroun
    position absolute
    left 0
    top:0
    width: 100%
    height:100%
    z-index: -1
    filter blur(10px)
</style>
