<template>
  <div id="app">
    <v-header :seller = 'seller'></v-header>
    <!-- <router-view/> -->
    <div class="nav">
      <div class="nav-item">
        <router-link to='/goods'>商品</router-link>
      </div>
      <div class="nav-item">
        <router-link to='/seller'>评价</router-link>
      </div>
      <div class="nav-item">
        <router-link to='/ratings'>商家</router-link>
      </div>
    </div>
        <router-view :seller = 'seller'></router-view>
  </div>
</template>

<script>
import header from './components/header/header.vue'
var ErrOk = 0
export default {
  data () {
    return {
      seller: {}
    }
  },
  created () {
    this.$http.get('/api/seller').then(response => {
      response = response.body
      console.log(response)
      if (response.errno === ErrOk) {
        this.seller = response.data
      }
    })
  },
  name: 'App',
  components: {
    'v-header': header
  }
}
</script>

<style lang="stylus" scoped>
@import './common/stylus/mixin.styl'

#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
.nav {
  display: flex;
  width: 100%;
  height:40px;
  border-1px(rgba(7,17,27,0.1))
}
.nav-item{
  flex: 1;
  line-height: 40px;
  text-align: center;
}
.nav-item .active {
  color:rgb(240,20,30)
}
</style>
