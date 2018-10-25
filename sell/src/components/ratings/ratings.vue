<template>
    <div class="ratings" ref="ratings">
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
              <star :size="36" :score="seller.foodScore"></star>
              <span class="score">{{seller.foodScore}}</span>
            </div>
            <div class="delivery-wrap">
              <span class="title">送达时间</span>
              <span class="delivery-time">{{seller.deliveryTime}}分钟</span>
            </div>
          </div>
        </div>
        <split></split>
        <selectrating @select="select" @toggleContent="toggleContent" :selectType="selectType" :onlyContent="onlyContent"
         :ratings="ratings"></selectrating>

        <div class="rating-wrap">
          <ul>
            <li v-show="needShow(rating.rateType,rating.text)" v-for="rating in ratings" class="rating-item">
              <div class="avatar">
                <img :src="rating.avatar" alt="小头像" width="28" height="28">
              </div>
              <div class="content">
                <h1 class="name">{{rating.username}}</h1>
                <div class="star-wrap">
                  <star :size="24" :score="rating.score"></star>
                  <span class="delivery" v-show="rating.delivery">{{rating.deliveryTime}}</span>
                </div>
                <p class="text">{{rating.text}}</p>
                <div class="recommend" v-show="rating.recommend && rating.recommend.length">
                  <span class="icon-thumb_up"></span>
                  <span v-for="item in rating.recommend" class="item">{{item}}</span>
                </div>
                <div class="time">
                  {{rating.rateTime | formatDate}}
                </div>
              </div>
            </li>
          </ul>
        </div>
      </div>
    </div>
</template>

<script>
const ALL = 2
const errOk = 0

import BScroll from 'better-scroll'
import star from '../../components/star/star'
import split from '../../components/split/split'
import selectrating from '../../components/selectrating/selectrating'
export default {
  props: {
    seller: {
      type: Object
    }
  },
  components: {
    star,
    split,
    selectrating
  },
  data () {
    return {
      ratings: [],
      selectType: ALL,
      onlyContent: true
    }
  },
  filters: {
    formatDate (time) {
      let date = new Date(time)
      // console.log(date) 格式化日期
      return date.getFullYear()+"-"+date.getMonth()+"-"+date.getDay()+"  "+date.getHours()+":"+date.getMinutes()+":"+date.getMilliseconds()
    }
  },
  created () {
    this.$http.get('/api/ratings').then((res) => { // 从json 获取到goods
      res = res.body
      if (res.errno === errOk) {
         this.ratings = res.data
         this.$nextTick(() => { // 因为vue的异步操作原因，当我们在create中操作dom时，注意在$nextTick
          this.scroll = new BScroll (this.$refs.ratings,{
            click: true
          })
        })
      }
    })
  },
  methods: {
    needShow (type, text) {
      if (this.onlyContent  && !text) { //
        return false
      }
      if (this.selectType === ALL) { //
        return true
      }else {
        return type ===this.selectType // 选择类型与数据一致
      }
    },
    select (type) {
      this.selectType = type
      /* this.$nextTick(() => {
         this.scroll.refresh()
       })*/
    },
    toggleContent () {
      this.onlyContent = !this.onlyContent
      /*  this.$nextTick(() => {
          this.scroll.refresh()
        })*/
    }
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

    .rating-wrap
      padding:0 18px
      .rating-item
        display flex
        padding:0 18px
        border-bottom 1px solid rgba(7,17,27,.1)
        .avatar
          flex 0,0,28px
          margin-right 12px
          width:28px
          img
            border-radius 50%
        .content
          position relative
          flex 1
          .name
            line-height 12px
            color: rgb(7,17,27)
            margin-bottom 4px
          .star-wrap
            margin-bottom 6px
            font-size 0
            .star
              display: inline-block
              margin-right 6px
              vertical-align top
            .delivery
              display inline-block
              color: rgb(147,153,159)
          .text
            margin-bottom 8px
            line-height:18px
            color: rgb(7,17,27)
            font-size 12px
          .recommend
            line-height 16px
            font-size 0
            .icon-thumb_up,.item
              display inline-block
              margin 0 8px 0 6px
              font-size 12px
            .icon-thumb_up
              color: rgb(0,160,220)
            .item
              padding: 0 6px
              border 1px solid  rgba(7,17,27,.1)
              border-radius 1px
              background #ffffff
              color: rgb(147,153,159)
          .time
            position absolute
            top:0
            right: 0
            line-height 12px
            font-size 10px
            color: rgb(147,153,159)
</style>
