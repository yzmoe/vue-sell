<template>
<div id="app">
  <v-header :seller="seller"></v-header>
  <div class="tab-list">
    <div class="tab-item">
      <router-link to="/goods">商品</router-link>
    </div>
    <div class="tab-item">
      <router-link to="/ratings">评论</router-link>
    </div>
    <div class="tab-item">
      <router-link to="/seller">商家</router-link>
    </div>
  </div>
  <keep-alive>
    <router-view :seller="seller" :goods="goods" :ratings="ratings"></router-view>
  </keep-alive>
</div>
</template>

<script>
import header from 'components/header/header'
import 'assets/reset.css'
import axios from 'axios'

export default {
  name: 'app',
  components: {
    'v-header': header
  },
  data: function() {
    return {
      goods:[],
      ratings:[],
      seller: {}
    }
  },
  created: function() {
    axios.get('../static/data.json')
      .then((response) => {
        this.goods=response.data.goods
        this.ratings=response.data.ratings
        this.seller = response.data.seller
        // console.log(this.goods)
        // console.log(this.ratings)
        // console.log(this.seller)
      })
      .catch((error) => {
        console.log(error);
      });
  }
}
</script>

<style>
.tab-list {
  display: flex;
  width: 100%;
  height: 40px;
}

.tab-item {
  flex-grow: 1;
  text-align: center;
  position: relative;
}


/*一像素边框*/

.tab-item::after {
  content: '';
  display: block;
  width: 100%;
  position: absolute;
  left: 0;
  bottom: 0;
  border-top: 1px solid rgba(7, 17, 27, 0.1);
}

@media (min-device-pixel-ratio:2) {
  .tab-item::after {
    transform: translateY(0.5);
  }
}

@media (min-device-pixel-ratio:3) {
  .tab-item::after {
    transform: translateY(0.33);
  }
}

.tab-item a {
  display: block;
  font-size: 14px;
  text-decoration: none;
  color: rgb(77, 85, 93);
  line-height: 40px;
}

.tab-item a.active {
  color: rgb(240, 20, 20)
}
</style>
