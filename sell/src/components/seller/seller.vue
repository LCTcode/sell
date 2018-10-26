<template>
    <div class="seller" ref="seller">
      <div class="seller-content">
        <div class="overview">
          <h1 class="title">{{seller.name}}</h1>
          <div class="desc">
            <star :size="36" :score="seller.score"></star>
            <span class="text">({{seller.ratingCount}})</span>
            <span class="text">月售{{seller.sellCount}}单</span>
          </div>
          <ul class="remark">
            <li class="block">
              <h2>起送价</h2>
              <div class="content">
                <span class="stress-price">{{seller.minPrice}}</span>
              </div>
            </li>
            <li class="block">
              <h2>商家配送</h2>
              <div class="content">
                <span class="stress-price">{{seller.deliveryPrice}}</span>
              </div>
            </li>
            <li class="block">
              <h2>平均配送时间</h2>
              <div class="content">
                <span class="stress-price">{{seller.deliveryTime}}</span>
              </div>
            </li>
          </ul>
        </div>
        <split></split>
        <div class="bulletin">
          <h1 class="title">公告与活动</h1>
          <div class="content-wrap">
            <p class="content">{{seller.bulletin}}</p>
          </div>
          <ul v-if="seller.supports" class="supports">
            <li v-for="(item,index) in seller.supports" :key="index" class="support-item">
              <span class="icon" :class="classMap[seller.supports[index].type]"></span>
              <span class="text">{{seller.supports[index].description}}</span>
            </li>
          </ul>
        </div>
        <split></split>
        <div class="pics">
          <h1 class="title">商家实景</h1>
          <div class="pic-wrap" ref="picWrap">
            <ul class="pic-list" ref="picList">
              <li class="pic-item" v-for="pic in seller.pics">
                <img :src="pic" alt="商家图片" width="120" height="90" >
              </li>
            </ul>
          </div>
        </div>
        <split></split>
        <div class="info">
          <h1 class="title">商家信息</h1>
          <ul>
            <li v-for="info in seller.infos" class="info-item">
              {{info}}
            </li>
          </ul>
        </div>
      </div>
    </div>
</template>

<script>
import BScroll from 'better-scroll'
import star from '../../components/star/star'
import split from '../../components/split/split'
export default {
  props: {
    seller: {
      type: Object
    }
  },
/*  data () {
    return {
      classMap: []
    }多余操作
  },*/
  created () {
      this.classMap = ['decrease', 'discount', 'guarantee', 'invoice', 'special'] // 巧妙 classMap[seller.supports[0].type]
    },
  watch: {
    'seller'() {
      this.$nextTick(() => {
        this._initPics()
      })
    }
  },
    mounted() {
      this.scroll = new BScroll(this.$refs.seller, {
        click: true
      })
      this.$nextTick(() => {
        this._initPics()
      })
    },
    methods: {
      _initPics() { // 图片滚动参考good页面的滚动
        if (this.seller.pics) {
          let picWidth = 120
          let margin = 6
          let width = (picWidth + margin) * this.seller.pics.length - margin
          this.$refs.picList.style.width = width + 'px'
          this.$nextTick(() => {
            if (!this.picScroll) {
              this.picScroll = new BScroll(this.$refs.picWrap, {
                scrollX: true,
                eventPassthrough: 'vertical' // 我们希望横向模拟横向滚动，而纵向的滚动还是保留原生滚动，我们可以设置 eventPassthrough 为 vertical
              })
            } else {
              this.picScroll.refresh()
            }
          })
        }
      }
    },
    components: {
      star,
      split
    }
  }
</script>

<style scoped lang="stylus" type="text/stylus">
  @import "../../common/stylus/mixin.styl"

  .seller
    position absolute
    top:174px
    left: 0
    bottom 0
    width: 100%
    overflow hidden
    .overview
      padding:18px
      .title
        margin-bottom 8px
        margin-left 18px
        color: rgb(7,17,27)
        font-size 14px
      .desc
        padding:18px
        line-height 18px
        font-size 0
        border 1px solid rgba(7,17,27,.1)
        .star
          display inline-block
          margin-right 8px
        .text
          display inline-block
          margin-right 12px
          font-size 10px
          color: rgb(77,85,93)
          vertical-align top
      .remark
        display flex
        margin-top 18px
        .block
          flex:1
          text-align center
          border-right 1px solid rgba(7,17,27,.1)
          &:last-child
            border-right 0px solid rgba(7,17,27,.1)
          h2
            margin-bottom 4px
            line-height 10px
            font-size 10px
            color: rgb(7,17,27)
          .content
            line-height 24px
            font-size 10px
            color: rgb(7,17,27)
            .stress-price
              font-size 24px
              padding-right 16px

    .bulletin
      padding:18px 18px 0 18px
      .title
        margin-bottom 8px
        margin-left 18px
        color: rgb(7,17,27)
        font-size 14px
      .content-wrap
        padding:10px 12px 16px 12px
        border 1px solid rgba(7,17,27,.1)
        .content
          line-height 24px
          font-size 12px
          color: red
      .supports
        .support-item
          padding 16px 12px
          font-size 0
          border 1px solid rgba(7,17,27,.1)
          .icon
            display inline-block
            width:16px
            height:16px
            margin-right 6px
            background-size 12px
            background-repeat no-repeat
            vertical-align top
            &.decrease
              bg-image("decrease_4")
            &.discount
              bg-image("discount_4")
            &.guarantee
              bg-image("guarantee_4")
            &.invoice
              bg-image("invoice_4")
            &.special
              bg-image("special_4")
          .text
            font-size 12px
            line-height 16px
            color: rgb(7,17,27)
    .pics
      padding:18px
      .title
        margin-bottom 12px
        margin-left 18px
        color: rgb(7,17,27)
        font-size 14px
      .pic-wrap
        width: 100%
        overflow hidden
        white-space nowrap
        .pic-list
          font-size 0
          .pic-item
            margin-right 6px
            display inline-block
            width:120px
            height:90px
            &:last-child
              margin-right 0
    .info
      padding:18px 18px 0 18px
      .title
        margin-bottom 12px
        margin-left 18px
        color: rgb(7,17,27)
        font-size 14px
        border-bottom 1px solid rgba(7,17,27,.1)
      .info-item
        padding:16px 12px
        color: rgb(7,17,27)
        font-size 12px
        border 1px solid rgba(7,17,27,.1)
</style>
