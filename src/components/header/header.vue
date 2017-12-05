<template lang="html">
  <div class="header">
    <div class="content-wrap">
      <div class="avatar">
        <img :src="seller.avatar" width="64" height="64">
      </div>
      <div class="content">
        <div class="tittle">
          <span class="brand"></span>
          <span class="name">{{seller.name}}</span>
        </div>
        <div class="disc">
          {{seller.description}}/{{seller.deliveryTime}}分钟送达
        </div>
        <div class="support" v-if="seller.supports">
          <span class="icon" :class="classMap[seller.supports[0].type]"></span>
          <span class="text">{{seller.supports[0].description}}</span>
        </div>
      </div>
      <div class="support-count" v-if="seller.supports" @click="showDetail">
        <span class="count">{{seller.supports.length}}个</span>
        <span class="icon-keyboard_arrow_right"></span>
      </div>
    </div>
    <div class="bulletin-wrap" @click="showDetail">
      <span class="bulletin-title"></span><span class="bulletin-text">{{seller.bulletin}}</span>
      <span class="icon-keyboard_arrow_right"></span>
    </div>
    <div class="background">
      <img :src="seller.avatar" width="100%"  height="100%">
    </div>
    <transition name="fade">
    <div class="detail" v-show="detailShow">
      <div class="detail-wrap">
        <div class="detail-main">
          <h1 class="name">{{seller.name}}</h1>
          <div class="star-wrap">
            <star :size="48" :score="seller.score"></star>
          </div>
          <div class="title">
            <div class="line"></div>
            <div class="text">
              优惠信息
            </div>
            <div class="line"></div>
          </div>
          <ul v-if="seller.supports" class="supports">
            <li class="supports-item" v-for='(key,index) in seller.supports'>
                <span class="icon" :class="classMap[seller.supports[index].type]"></span>
                <span class='text'>{{seller.supports[index].description}}</span>
            </li>
          </ul>
          <div class="title">
            <div class="line"></div>
            <div class="text">
              商家公告
            </div>
            <div class="line"></div>
          </div>
          <div class="bulletin">
            <p class="content">{{seller.bulletin}}</p>
          </div>
        </div>
      </div>
      <div class="detail-close" @click="hideDetail">
        <span class="icon-close"></span>
      </div>
    </div>
  </transition>
  </div>
</template>

<script>
import star from 'components/star/star'
export default {
  name: 'header',
  props: {
    seller: {
      type: Object
    }
  },
  data: function() {
    return {
      detailShow: false
    }
  },
  methods: {
    showDetail: function() {
      this.detailShow = true
    },
    hideDetail:function(){
      this.detailShow = false
    }
  },
  created: function() {
    this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee']
  },
  components:{star}
}
</script>

<style lang="css">
.header{
  color: #fff;
  background-color: rgba(7, 17, 27, 0.5);
  position: relative;
  overflow: hidden;
}
.header .content-wrap{
  padding: 24px 12px 18px 24px;
  font-size: 0;
  position: relative;
}
.header .content-wrap .avatar,.content-wrap .content{
  display: inline-block;
}
.header .content-wrap .avatar{
  border-radius: 2px;
  overflow: hidden;
  vertical-align: top;
}
.header .content-wrap .content{
  font-size: 14px;
  margin-left: 16px;
}
.header .content-wrap .content .tittle{
  margin: 2px 0 8px;
}
.header .content-wrap .content .tittle .brand{
  display: inline-block;
  width: 30px;
  height: 18px;
  background-image: url('brand@2x.png');
  background-size: cover;
  vertical-align: top;
}
@media (min-device-pixel-ratio:3) {
  .content-wrap .content .tittle .brand{
    background-image: url('brand@3x.png');
  }
}
.header .content-wrap .content .tittle .name{
  margin-left: 6px;
  font-size: 16px;
  font-weight: bold;
  line-height: 18px;
  vertical-align: top;
}


.header .content-wrap .content .desc{
  margin-bottom: 10px;
  line-height: 12px;
  font-size: 12px;
}

.header .content-wrap .content .support{
  margin-top: 10px
}
.header .content-wrap .content .support .icon{
  display: inline-block;
  width: 12px;
  height: 12px;
  margin-right: 4px;
  background-size: cover;
  vertical-align: top;
}
.header .content-wrap .content .support .decrease{
  background-image: url('decrease_1@2x.png');
}
.header .content-wrap .content .support .discount{
  background-image: url('discount_1@2x.png');
}
.header .content-wrap .content .support .guarantee{
  background-image: url('guarantee_1@2x.png');
}
.header .content-wrap .content .support .invoice{
  background-image: url('invoice_1@2x.png');
}
.header .content-wrap .content .support .special{
  background-image: url('special_1@2x.png');
}
@media (min-device-pixel-ratio:3){
  .content-wrap .content .support .decrease{
    background-image: url('decrease_1@3x.png');
  }
  .content-wrap .content .support .discount{
    background-image: url('discount_1@3x.png');
  }
  .content-wrap .content .support .guarantee{
    background-image: url('guarantee_1@3x.png');
  }
  .content-wrap .content .support .invoice{
    background-image: url('invoice_1@3x.png');
  }
  .content-wrap .content .support .special{
    background-image: url('special_1@3x.png');
  }
}
.header .content-wrap .content .support .text{
  line-height: 12px;
  font-size: 10px;
  vertical-align: top;
}
.header .content-wrap .support-count{
  position: absolute;
  right: 12px;
  bottom: 18px;
  padding: 0 8px;
  height: 24px;
  line-height: 24px;
  border-radius: 14px;
  background-color: rgba(0, 0, 0, 0.2);
  text-align: center;
}
.header .content-wrap .support-count *{
  font-size: 10px;
}
.header .content-wrap .support-count .icon-keyboard_arrow_right{
  line-height: 24px;
  margin-left: 2px;
  /*vertical-align: top;*/
}


/*公告部分开始*/
.header .bulletin-wrap{
  height: 28px;
  line-height: 28px;
  padding: 0 22px 0 12px;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  position: relative;
  background-color: rgba(7, 17, 27, 0.2);
}
.header .bulletin-wrap .bulletin-title{
  display: inline-block;
  width: 22px;
  height: 12px;
  margin-top: 8px;
  background-image: url('bulletin@2x.png');
  background-size: cover;
}
@media (min-device-piexl-ratio:3) {
  .header .bulletin-wrap .bulletin-title{
    background-image: url('bulletin@3x.png');
  }
}
.header .bulletin-wrap .bulletin-text{
  margin: 0 4px;
  font-size: 10px;
  line-height: 28px;
  vertical-align: top;
}
.header .bulletin-wrap .icon-keyboard_arrow_right{
  position: absolute;
  font-size: 10px;
  right: 12px;
  top: 8px;
}

/*header部分背景*/
.header .background{
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: -1;
  filter: blur(10px);
}

/*header弹出层*/
.header .detail{
  position: fixed;
  top: 0;
  left: 0;
  z-index: 999;
  width: 100%;
  height: 100%;
  overflow: auto;
  opacity: 1;
  background-color: rgba(7, 17, 27, 0.8);
}
.header .fade-enter-active,.header .fade-leave-active{
  transition: all 0.65s
}
.header .fade-enter,.header .fade-leave-active{
  opacity: 0;
  background: rgba(7, 17, 27, 0)}
.header .detail .detail-wrap{
  min-height: 100%;
  margin-bottom: -64px;
}
.header .detail .detail-wrap::before{
  content: '';
  display: block;
  height:64px;
}
.header .detail .detail-wrap::after{
  content: '';
  display: block;
  height: 64px;
}
.header .detail .detail-wrap .detail-main .name{
  line-height: 16px;
  font-size: 16px;
  font-weight: 700;
  text-align: center;
}
.header .detail .detail-wrap .detail-main .star-wrap{
  margin-top: 16px;
  padding: 2px 0;
  text-align: center;
}
.header .detail .detail-wrap .detail-main .title{
  display: flex;
  width:80%;
  margin: 30px auto 24px;
}
.header .detail .detail-wrap .detail-main .title .line{
  flex-grow: 1;
  border-bottom: 1px solid rgba(255,255,255, 0.2);
  position: relative;
  top: -6px;
}
.header .detail .detail-wrap .detail-main .title .text{
  padding: 0 12px;
  font-size: 14px;
  font-weight: 700;
}
.header .detail .detail-wrap .detail-main .supports{
  width: 80%;
  margin: 0 auto;
}
.header .detail .detail-wrap .detail-main .supports .supports-item{
  padding: 0 12px;
  margin-bottom: 12px;
  font-size: 0;
}
.header .detail .detail-wrap .detail-main .supports .supports-item:last-child{
  margin-bottom: 0;
}
.header .detail .detail-wrap .detail-main .supports .supports-item .icon{
  display: inline-block;
  width: 16px;
  height: 16px;
  margin-right: 6px;
  background-size: cover;
  vertical-align: top;
}
.header .detail .detail-wrap .detail-main .supports .supports-item .decrease{
  background-image: url('decrease_2@2x.png');
}
.header .detail .detail-wrap .detail-main .supports .supports-item .discount{
  background-image: url('discount_2@2x.png');
}
.header .detail .detail-wrap .detail-main .supports .supports-item .guarantee{
  background-image: url('guarantee_2@2x.png');
}
.header .detail .detail-wrap .detail-main .supports .supports-item .invoice{
  background-image: url('invoice_2@2x.png');
}
.header .detail .detail-wrap .detail-main .supports .supports-item .special{
  background-image: url('special_2@2x.png');
}
@media (min-device-pixel-ratio:3){
  .header .detail .detail-wrap .detail-main .supports .supports-item .decrease{
    background-image: url('decrease_2@3x.png');
  }
  .header .detail .detail-wrap .detail-main .supports .supports-item .discount{
    background-image: url('discount_2@3x.png');
  }
  .header .detail .detail-wrap .detail-main .supports .supports-item .guarantee{
    background-image: url('guarantee_2@3x.png');
  }
  .header .detail .detail-wrap .detail-main .supports .supports-item .invoice{
    background-image: url('invoice_2@3x.png');
  }
  .header .detail .detail-wrap .detail-main .supports .supports-item .special{
    background-image: url('special_2@3x.png');
  }
}
.header .detail .detail-wrap .detail-main .supports .supports-item .text{
  font-size: 12px;
  line-height: 16px;
}
.header .detail .detail-wrap .detail-main .bulletin{
  width: 80%;
  margin: 0 auto;
}
.header .detail .detail-wrap .detail-main .bulletin .content{
  padding: 0 12px;
  line-height: 24px;
  font-size: 12px;
}
.header .detail .detail-close{
  width: 32px;
  height: 32px;
  margin: 0 auto;
  font-size: 32px;
}
</style>
