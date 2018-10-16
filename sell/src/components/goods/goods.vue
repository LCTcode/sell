<template>
    <div class="goods">
      <div class="menu-wrapper">
        <ul>
          <li v-for="(item,index) in goods" :key="index" class="menu-item">
            <span class="text">
              <span v-show="item.type>0" class="icon" :class="classMap[item.type]"></span>
            {{item.name}}</span>
          </li>
        </ul>
      </div>
      <div class="foods-wrapper">
        <ul>
          <li v-for="(item,index) in goods" class="food-list" :key="index">
            <h1 class="title">{{item.name}}</h1>
              <ul>
                <li v-for="(food,index) in item.foods" class="food-item" :key="index">
                  <div class="icon">
                    <img :src="food.icon" alt="" width="57px">
                  </div>
                  <div class="content">
                    <h2 class="name">
                      {{food.name}}
                    </h2>
                    <p class="desc">
                      {{food.description}}
                    </p>
                    <div class="estra">
                      <span class="count">月售{{food.sellCount}}份</span>
                      <span>好评率{{food.rating}}%</span>
                    </div>
                    <div class="price">
                      <span class="now">￥{{food.price}}</span>
                      <span class="old" v-show="food.oldPrice">￥{{food.oldPrice}}</span>
                    </div>
                  </div>
                </li>
              </ul>
          </li>
        </ul>
      </div>
    </div>
</template>

<script>
const errOk = 0
export default {
  props: {
    seller: {
      type: Object
    }
  },
  data () {
    return {
      goods: {}
    }
  },
  created () {
    this.$http.get('/api/goods').then((res) => {
      res = res.body
      if (res.errno === errOk) {
        this.goods = res.data
      }
    })
    this.classMap = ['decrease', 'discount', 'guarantee', 'invoice', 'special']
  }
}
</script>

<style scoped lang="stylus"  type="text/stylus">
  @import "../../common/stylus/mixin.styl"
  .goods
    display flex
    position absolute
    bottom:46px
    top:174px
    width: 100%
    overflow hidden
    .menu-wrapper
      flex 0 0 80px
      width 80px
      background #f3f5f7
      .menu-item
        display table
        height:54px
        width:56px
        padding 0 12px // 80-56)/2 居中
        .icon
          display inline-block
          width:12px
          height:12px
          background-size 12px 12px
          margin-right 2px
          background-repeat no-repeat
          &.decrease
            bg-image("decrease_3")
          &.discount
            bg-image("discount_3")
          &.guarantee
            bg-image("guarantee_3")
          &.invoice
            bg-image("invoice_3")
          &.special
            bg-image("special_3")
        .text
          font-size 12px
          display table-cell
          width:56px
          vertical-align middle
          font-size 12px
          border-bottom 1px solid rgba(7,17,27,.1)
    .foods-wrapper
      flex 1
      background #ffffff
      .title
        padding-left:14px
        height:26px
        line-height 26px
        border-left 2px solid #d9dde1
        font-size 12px
        color: rgb(147,153,159)
        background #f3f5f7
      .food-item
        display flex
        margin 18px
        border-bottom 1px solid rgba(7,17,27,.1)
        padding-bottom 18px
        &:last-child
          border 0px solid
          margin-bottom 0
        .icon
          flex: 0 0 57px
          margin-right 10px
        .content
          flex:1
          .name
            margin 2px 0 8px 0
            height:14px
            line-height 14px
            font-size 14px
            color: rgb(7,17,27)
          .desc,.estra
            line-height 12px
            font-size 10px
            height:12px
            color: rgb(7,17,27)
          .desc
            margin-bottom 8px
          .estra
            line-height 10px
            &.count
             margin-right 12px
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
</style>
