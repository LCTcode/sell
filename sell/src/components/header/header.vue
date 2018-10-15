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
      <div class="supports-count" v-if="seller.supports"  @click="showDetail">
        <div class="count">{{seller.supports.length}}个</div>
        <i class="icon-keyboard_arrow_right"></i>
      </div>
    </div>
    <div class="bulletin-wrapper"  @click="showDetail">
      <span class="bulletin-title"></span><!--
      --><span class="bulletin-text">{{seller.bulletin}}</span>
      <i class="icon-keyboard_arrow_right"></i>
    </div>
    <div class="backgroun">
      <img :src="seller.avatar" alt="" width="100%" height="100%">
    </div>
    <transition name="fade">
    <div class="detail" v-show="detailShow" >
          <div class="detailwrap clearfix">
            <div class="detail-main">
             <h1 class="detail-name">{{seller.name}}</h1>
             <div class="star-warpper">
                <star :size="48" :score="seller.score"></star> <!--星星组件-->
             </div>
              <div class="title">
                <div class="line"></div>
                <div class="text">优惠信息</div>
                <div class="line"></div>
              </div>
              <ul v-if="seller.supports" class="supports">
                <li v-for="(item,index) in seller.supports" :key="index" class="support-item">
                  <span class="icon" :class="classMap[seller.supports[index].type]"></span>
                  <span class="text">{{seller.supports[index].description}}</span>
                </li>
              </ul>
              <div class="title">
                <div class="line"></div>
                <div class="text">商家公告</div>
                <div class="line"></div>
              </div>
              <div class="shop-container">
                <p class="shop-content">
                  Lorem ipsum dolor sit amet, consectetur adipisicing elit. Atque earum fugit illum ipsum nihil, perspiciatis qui sint tempore. Ipsam, neque.
                </p>
              </div>
            </div>
          </div>

            <div class="detail-close"  @click="closeDetail">
              <i class=" icon-close "></i>
            </div>
    </div>
    </transition>
  </div>
</template>

<script>
import star from '../../components/star/star.vue'
export default {
  props: {
    seller: {
      type: Object
    }
  },
  data () {
    return {
      detailShow: false
    }
  },
  methods: {
    showDetail () {
      this.detailShow = true
    },
    closeDetail () {
      this.detailShow = false
    }
  },
  created () {
    this.classMap = ['decrease', 'discount', 'guarantee', 'invoice', 'special'] // 巧妙 classMap[seller.supports[0].type]
  },
  components: {
    star
  }
}
</script>

<style lang="stylus"  type="text/stylus">
  @import "../../common/stylus/mixin.styl"
  @import "../../common/stylus/base.styl"
  @import "../../common/stylus/style.styl"
  .header
    position relative
    color: #fff
    background rgba(7,17,27,.5)
    overflow hidden //去除背景图层模糊超出的部分
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
      top:8px

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
      top:7px

  .backgroun
    position absolute
    left 0
    top:0
    width: 100%
    height:100%
    z-index: -1
    filter blur(10px)

  .fade-enter-active,.fade-leave-active
    transition all .5s // 动画 已经和vue 1.x 不一样了
    opacity .8
    background rgba(7,17,27,.8)
  .fade-enter,.fade-leave-active
    opacity 0
    background rgba(7,17,27,0)
  .detail  // 弹出图层信息
    position fixed
    top: 0
    right: 0
    z-index: 100
    width:100%
    height:100%
    overflow auto
    background rgba(7,17,27,0.8)
    -webkit-backdrop-filter blur(10px) // 背景模糊
    .detailwrap
      min-height: 100%
      width:100%
      .detail-main
        margin-top: 64px
        padding-bottom: 64px
        .detail-name
          line-height 16px
          font-size 16px
          text-align center
        .star-warpper
          margin-top 18px
          text-align center
          padding:2px 0
        .title
          display flex
          width: 80%
          margin 28px auto 24px auto
          .line
            flex 1
            position relative
            top: -6px
            border-bottom 1px solid rgba(255,255,255,.2)
          .text
            padding:0 12px
            font-size 14px
        .supports
          width: 80%
          margin 0 auto
          .support-item
            padding:0 12px
            margin-bottom 12px
            font-size 0
            &:last-child
              margin-bottom 0
            .icon
              display inline-block
              width:16px
              height:16px
              background-size 16px 16px
              vertical-align top
              margin-right 16px
              background-repeat no-repeat
              &.decrease
                bg-image("decrease_2")
              &.discount
                bg-image("discount_2")
              &.guarantee
                bg-image("guarantee_2")
              &.invoice
                bg-image("invoice_2")
              &.special
                bg-image("special_2")
            .text
              line-height 16px
              font-size 12px
        .shop-container
          width: 100%
          margin 0 auto
          .shop-content
            padding:0 24px
            line-height 24px
            font-size 12px
    .detail-close // 弹窗关闭
        position relative
        width:32px
        height:32px
        margin: -64px auto 0 auto
        clear both
        font-size 32px
</style>
