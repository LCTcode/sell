<template>
    <div class="ratings">
      <div class="ratings-content">
        <div class="overview">
          <div class="overview-left">
            <h1 class="score">{{seller.score}}</h1>
            <div class="title">综合评分</div>
            <div class="rank">高于周边商家{{seller.rankRate}}%</div>
          </div>
          <div class="overview-right">
            <div class="score-wrap">
              <span class="title">服务态度</span>
              <star :size="36" :score="seller.serviceScore"></star>
              <span class="score">{{seller.serviceScore}}</span>
            </div>
            <div class="score-wrap">
              <span class="title">商品评分</span>
              <star :size="36" :score="seller.foodScore" class="star"></star>
              <span class="score">{{seller.foodScore}}</span>
            </div>
            <div class="delivery-wrap">
              <span class="title">送达时间</span>
              <span class="delivery-time">{{seller.deliveryTime}}分钟</span>
            </div>
          </div>
        </div>
      </div>
    </div>
</template>

<script>
import star from '../../components/star/star'
export default {
  props: {
    seller: {
      type: Object
    }
  },
  components: {
    star
  }
}
</script>

<style scoped lang="stylus" type="text/stylus">
  .ratings
    position absolute
    top:174px
    left: 0
    bottom 0
    width: 100%
    overflow hidden
    .overview
      display flex
      padding:18px 0
      .overview-left
        flex:0,0,137px
        width:137px
        border-right 1px solid rgba(7,17,27,.1)
        text-align center
        padding: 6px 0
        @media only screen and (max-width 320px)
          flex 0,0,120px
          width:120px
        .score
          line-height 28px
          color: rgb(255,153,0)
          font-size 24px
          margin-bottom 6px
        .title
          line-height 12px
          font-size 12px
          color: rgb(7,17,27)
          margin-bottom 8px
        .rank
          line-height 10px
          font-size 10px
          color:rgb(147,153,159)
      .overview-right
        flex 1
        width: 100%
        padding-left 24px
        @media only screen and (max-width 320px)
          padding-left 6px
        .score-wrap
          margin-bottom 8px
          font-size 0
          .title
            display inline-block
            font-size 12px
            color: rgb(7,17,27)
            line-height 18px
          .star
            display inline-block
            margin 0 12px
          .score
            font-size 12px
            display inline-block
            color: rgb(255,153,0)
            vertical-align top
            line-height 18px
        .delivery-wrap
          font-size: 0
          .title
            line-height 18px
            font-size 12px
            color: rgb(7,17,27)
          .delivery-time
            margin-left 12px
            font-size 12px
            color: rgb(147,153,159)

</style>
