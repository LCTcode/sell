# sell

> A Vue.js project

## Build Setup

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build

# build for production and view the bundle analyzer report
npm run build --report
```

For a detailed explanation on how things work, check out the [guide](http://vuejs-templates.github.io/webpack/) and [docs for vue-loader](http://vuejs.github.io/vue-loader).



因为原项目是很久之前的，所以算是重新对项目进行了一些重构。原项目采用虚拟数据为配置dev-server.js， 而现在已经没有这个文件，现在的配置是在 webpack-de-config.js 中，配置方式参考：
<a href="https://cloud.tencent.com/developer/article/1195991">https://cloud.tencent.com/developer/article/1195991</a>  中的方法三。
webpack-de-config.js 我的设置：

const app = express()//请求server
var appData = require('../data.json')//加载本地数据文件
var seller = appData.seller//获取对应的本地数据
var goods = appData.goods
var ratings = appData.ratings
var apiRoutes = express.Router()
app.use('/api', apiRoutes)//通过路由请求数据

在devserver中添加before:
  before(app) {
      app.get('/api/seller', (req, res) => {
        res.json({
          errno: 0,
          data: seller
        })//接口返回json数据，上面配置的数据seller就赋值给data请求后调用
      }),
        app.get('/api/goods', (req, res) => {
          res.json({
            errno: 0,
            data: goods
          })
        }),
        app.get('/api/ratings', (req, res) => {
          res.json({
            errno: 0,
            data: ratings
          })
        })
    }

项目效果图片:</br>

<img src="https://github.com/LCTcode/sell/blob/master/sell/showPic/sell.gif" />

<strong>各个组件间关系</strong> ：

─app.vue
  │  ├──header.vue--头部组件
  │  │  ├──star.vue--星星评分组件
  │  ├──goods.vue--商品组件
  │  │  ├──shopcart.vue--购物车组件,包括小球飞入购物车动画
  │  │  ├──cartcontrol.vue--购买加减图标控件--选中数量返回给父组件goods，goods响应后，重新计算选中数量，将数据发送给购物车组件，
  │  │  ├──food.vue--商品详情页
  │  │  │  ├──ratingselect.vue--评价内容筛选组件
  │  ├──ratings.vue--评论组件
  │  │  ├──ratingselect.vue--评价内容筛选组件
  │  ├──seller.vue--商家组件

独立组件
  ├──split.vue--关于分割线组件




<strong>模拟数据的获取</strong> 我这里依旧采用 v-resource 目前官方推荐是 axios：

  
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
  }

如果要使用axios 可以 npm i axios 引入，然后 Vue.prototype.$http = axios 即可。

<strong>样式引用以及移动端 1px 解决</strong>：
样式初始化采用 reset.css , 1px 问题采用 border.css ,其中关于 移动端1px问题：可以查看张鑫旭大大的文章：https://www.zhangxinxu.com/wordpress/2012/08/window-devicepixelratio/

公式：设备上像素 = 样式像素 * 设备像素比   思路：media + scale 

通过查询它的设备像素比 devicePixelRatio
在设备像素比为1.5倍时, round(1px 1.5 / 0.7) = 1px 
在设备像素比为2倍时, round(1px 2 / 0.5) = 1px

比如：
@media (-webkit-min-device-pixel-ratio: 2), (min-device-pixel-ratio: 2) {
  .border-1px {
    &::after {
      -webkit-transform: scaleY(0.5);
      transform: scaleY(0.5);
    }


<strong>路由</strong>

vue1 与  vue2中的路由配置方式也有不同，这里采用vue2.
在router 目录下的index.js 中配置：

import Vue from 'vue'
import Router from 'vue-router'
import goods from '../components/goods/goods'
import ratings from '../components/ratings/ratings'
import seller from '../components/seller/seller'

Vue.use(Router)

export default new Router({
  linkActiveClass: 'active',
  routes: [
    {
      path: '/goods',
      component: goods
    },
    {
      path: '/ratings',
      component: ratings
    },
    {
      path: '/seller',
      component: seller
    },
    { // 重定向
      path: '/',
      redirect: '/goods'
    }
  ]
})
* 其中 linkActiveClass: 'active', 是在路由中添加样式，当路由激活时显示样式。

* 重定向是为了刷新以及开始时回到 goods 商品页



<strong>动画</strong>

在vue 2中，使用标签 <transition name="fade"> </transition>包裹模块，其中name="fade"可以自己定义，其会渲染时自动拓展为：
.fade-enter，.fade-enter-active等。

fade-enter
fade-enter-active
fade-leave
fade-leave-active

动画钩子函数：

before-enter
before-leave
before-appear
enter
leave
appear
after-enter
after-leave
after-appear
enter-cancelled
leave-cancelled (v-show only)
appear-cancelled




<strong>组件通信</strong>

父子间采用 props,  
子对父通信，需要子组件采用 $emit派发，然后才父组件使用v-on 也就是@ 进行监控接收。（使用场景有：有时候子组件需要修改父组件数据时）

例如项目中：
selectrating (子): this.$emit('select', type)

food (父)：@select="select" ，然后直接在methods 中定义select 进行数据变更。

非父子组件通信：
1：采用vuex
2：空实例bus 




<strong>获取 DOM</strong>
  
  
在标签中用 ref="" 标记，在sctipt 中用 this.$refs 接收


<strong>重点布局</strong>

1  flex 布局，参考阮一锋老师的：http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html
出现在项目中的左侧固定，右侧自适应（goods.vue中的左右部分）
首先父级：
.goods
    display flex
    
   左边子级：
   .menu-wrapper
      flex 0 0 80px
      width 80px
    
  右边子级：
  .foods-wrapper
      flex 1
      
2 sticky-footer布局
在 header 组件的详情页采用 sticky-footer 布局，主要特点是如果页面内容不够长的时候，页脚块粘贴在视窗底部；如果内容足够长时，页脚块会被内容向下推送。
比如 header.vue 中的商品详情弹出部分就采用这样的布局

 .detail  // 弹出图层信息
    position fixed 
    top: 0
    right: 0
    z-index: 100
    width:100%
    height:100%
    overflow auto // 内容超出时出现滚动条
    background rgba(7,17,27,0.8)
    -webkit-backdrop-filter blur(10px) // 背景模糊
    
    .detailwrap
      min-height: 100% // 内容不够时保证内容铺满
      width:100%

      
      
<strong>滚动 better-scroll 的使用</strong>


以 food 组件中为例子：

首先绑定DOM  <div class="food" v-show="showFlag" ref="foodsScroll">
  
         this.$nextTick(() => {
         if (!this.scroll) {
           this.scroll = new BScroll(this.$refs.foodsScroll, {
             click: true
           })
         }else {
           this.scroll.refresh()
         }
       })
       
       注意
       1 better-scroll 插件在移动端使用时需要设置 click：true，否则移动端滑动无效
       2 this.$nextTick(() 在dom 更新后立即执行
  
  better-scroll 文档：https://github.com/ustbhuangyi/better-scroll
  
  
  
<strong>过滤器</strong>

 过滤器设置时间格式：
 
 <div class="time">
   {{rating.rateTime | formatDate}}
 </div>
 
   filters: {
    formatDate (time) {
      let date = new Date(time)
      // console.log(date) 格式化日期
      return date.getFullYear()+"-"+date.getMonth()+"-"+date.getDay()+"                           "+date.getHours()+":"+date.getMinutes()+":"+date.getMilliseconds()
    }
  }
 
 
 
 <strong>让我觉得很巧妙的地方</strong>
 
 1 star 星星组件：
 
 绑定星星类型的class(48,36,24尺寸),使用starType
 
 starType () {
      return 'star-' + this.size
    },
    
 计算星星个数：
 
 itemClasses () {
      let result = []
      let score = Math.floor(this.score * 2) / 2
      console.log(score + '----1')
      let hasDecimal = score % 1 !== 0 // score有小数，则添加半颗星星
      let integer = Math.floor(score) // 整数个星星
      console.log(score + '----2')

      for (let i = 0; i < integer; i++) {
        result.push(CLS_ON)
        console.log('a---')
      }
      if (hasDecimal) {
        result.push(CLS_HALF)
        console.log('b===')
      }
      while (result.length < LENGTH) { // 将不足的地方添加0颗星星
        result.push(CLS_OFF)
        console.log('c+++')
      }
      return result
    }
  }
}

根据 starType 设置对应星星样式：

 &.star-48
      .star-item
        width:20px
        height:20px
        background-size 20px 20px
        margin-right 22px
        &:last-child
          margin-right 0
        &.on
          bg-image('star48_on')
        &.half
          bg-image('star48_half')
        &.ff
          bg-image('star48_off')
    &.star-36
      .star-item
        width:15px
        height:15px
        background-size 15px 15px
        margin-right 6px
        &:last-child
          margin-right 0
        &.on
          bg-image('star36_on')
        &.half
          bg-image('star36_half')
        &.ff
          bg-image('star36_off')
    &.star-24
      .star-item
        width:10px
        height:10px
        background-size 10px 10px
        margin-right 6px
        &:last-child
          margin-right 0
        &.on
          bg-image('star24_on')
        &.half
          bg-image('star24_half')
        &.ff
          bg-image('star24_off')

 
 其中bg-image 是mixin.styl
 
 bg-image($url)
  background-image: url($url+"@2x.png")
  @media (-webkit-min-device-pixel-ratio: 3),(min-device-min-device-pixel-ratio: 3)
    background-image: url($url+"@3x.png")

 
 2 selectrating 组件中子组件传值修改父组件数值，然后修改对应样式
 
 food 父组件传值：
  
  <selectrating @select="select" @toggleContent="toggleContent" :selectType="selectType" :onlyContent="onlyContent" :desc="desc"
                     :ratings="food.ratings"></selectrating>
             
 selectrating 子组件：
 
   <div class="rating-type">
          <span @click="select(2,$event)" class="block positive" :class="{'active':selectType===2}">{{desc.all}} <span class="count">{{ratings.length}}</span></span> 
  
          <span @click="select(0,$event)" class="block positive" :class="{'active':selectType===0}">{{desc.positive}}  <span class="count">{{positive.length}}</span></span>
          
          <span @click="select(1,$event)" class="block negative" :class="{'active':selectType===1}">{{desc.negative}}  <span class="count">{{negative.length}}</span></span>
        </div>
 
 
  methods:{
    select (type, event) {
      // this.selectValue = this.selectType
      if(!event._constructed) {
        return
      }
      // this.selectValue = type
      console.log(type)
      this.$emit('select', type)  传递给父组件

    },
    toggleContent (event) {
      if(!event._constructed) {
        return
      }
      this.$emit('toggleContent')
    }
 
 
 
 @click="select(2,$event)"   select方法传入类型和事件,然后在methods里面调用父组件的方法
 
:class="{'active':selectType ===2}"   根据类型来确定显示的class,实现不同类型显示不同样式的目的


  food 父组件中                   
                     
  @select="select" @toggleContent="toggleContent" 对其进行监控，同时：
  
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
    }, 
   对数据进行修改
 


 <strong>数据状态缓存</strong>

vue2 中为了保存路由切换时的状态以及数据，提供了<keep-alive> 标签。

   <keep-alive>
      <router-view :seller="seller" ></router-view> <!--路由传值-->
    </keep-alive>








