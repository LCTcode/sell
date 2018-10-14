<template>
  <div id="app">
    <v-header :seller="seller"></v-header> <!--获取数据后将数据绑定，在子组件用props接收-->
    <div class="nav">
      <div class="navItem"><router-link to="goods">商品</router-link></div>
      <div class="navItem"><router-link to="ratings">评论</router-link></div>
      <div class="navItem"><router-link to="seller">商家</router-link></div>
    </div>
    <router-view></router-view>
  </div>
</template>

<script>
import header from './components/header/header'

const errOk = 0

export default {
  data () {
    return {
      seller: {}
    }
  },
  created () {
    this.$http.get('/api/seller').then((res) => {
      res = res.body
      if (res.errno === errOk) {
        this.seller = res.data
        console.log(this.seller)
      }
    })
  },
  components: {
    'v-header': header
  }
}
</script>

<style>
  .nav{
    display: flex;
    width: 100%;
    height: 40px;
    line-height: 40px;
    border-bottom: 1px solid rgba(7,17,27,.1);
  }
  .navItem{
    flex: 1;
    text-align: center;
  }
  .navItem a{
    display: block;
    font-size: 14px;
    color: rgb(77,85,93);
  }
  .navItem .active{
    color: red;
  }
</style>
