<template>
    <div class="goods">
      <div class="menu-wrapper" ref="menuWrapper">
        <ul>
          <li v-for="(item,index) in goods" @click="selectMenu(index,$event)" :key="index" class="menu-item" :class="{'current': currentIndex === index}">
            <span class="text">
              <span v-show="item.type>0" class="icon" :class="classMap[item.type]"></span>
            {{item.name}}</span>
          </li>
        </ul>
      </div>
      <div class="foods-wrapper" ref="foodsWrapper">
        <ul>
          <li v-for="(item,index) in goods" class="food-list food-list-hook" :key="index">
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
                      <span class="count">月售{{food.sellCount}}份</span><!--
                     --><span>好评率{{food.rating}}%</span>
                    </div>
                    <div class="price">
                      <span class="now">￥{{food.price}}</span><!--
                      --><span class="old" v-show="food.oldPrice">￥{{food.oldPrice}}</span>
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
import BScroll from 'better-scroll'

const errOk = 0

export default {
  props: {
    seller: {
      type: Object
    }
  },
  data () {
    return {
      goods: [],
      listHeight: [], // 存储滚动的高度
      scrollY: 0
    }
  },
  computed: {
    currentIndex () {
      for (let i = 0; i < this.listHeight.length; i++) {
        let height1 = this.listHeight[i]
        let height2 = this.listHeight[i + 1]
        if (!height2 || (this.scrollY >= height1 && this.scrollY < height2)) {
          return i
        }
      }
      return 0
    }
  },
  created () {
    this.$http.get('/api/goods').then((res) => {
      res = res.body
      if (res.errno === errOk) {
        this.goods = res.data
        this.$nextTick(() => {
          this._initScroll()
          this._catchHeight()
        })
        // this.$nextTick(() => { // 因为vue的异步操作原因，当我们在create中操作dom时，注意在$nextTick
        //   this._initScroll()
        //   this._catchHeight()
        // })
      }
    })
    this.classMap = ['decrease', 'discount', 'guarantee', 'invoice', 'special']
  },
  methods: {
    selectMenu (index, event) {
      if (!event._constructed) { // 如果不存在这个属性,则为原生点击事件，不执行下面的函数
        return
      }
      let foodList = this.$refs.foodsWrapper.getElementsByClassName('food-list-hook')
      let el = foodList[index]
      this.foodsScroll.scrollToElement(el, 300)
    },
    _initScroll () {
      this.menuScroll = new BScroll(this.$refs.menuWrapper, {
        click: true
      }) // dom对象&json /BScroll 移动端默认阻止一些事件比如点击，所以要 click true

      this.foodsScroll = new BScroll(this.$refs.foodsWrapper, {
        probeType: 3 // better_scroll 当 probeType为 3 的时候，不仅在屏幕滑动的过程中，而且在 momentum 滚动动画运行过程中实时派发 scroll 事件
      })
      this.foodsScroll.on('scroll', (pos) => {
        this.scrollY = Math.abs(Math.round(pos.y))
      })
    },
    _catchHeight () { // 获取滚动高度
      let foodList = this.$refs.foodsWrapper.getElementsByClassName('food-list-hook') // 获取节点
      let height = 0
      this.listHeight.push(height)
      for (let i = 0; i < foodList.length; i++) { // 遍历节点内容
        let item = foodList[i]
        height += item.clientHeight // 节点是一个数组，将其遍历内容后的高度相加，以此获得节点高度
        this.listHeight.push(height)
      }
    }
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
        &.current // 滚动样式
          position relative
          margin-top 1px
          z-index:10
          background #ffffff
          font-weight 700
          color red
          .text
            color: red
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
          .desc, .estra
            line-height 10px
            font-size 10px
            height:12px
            color: rgb(7,17,27)
          .desc
            line-height 10px
            margin-bottom 8px
          .estra
            line-height 12px
            .count
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
