<template lang="html">
<div>
  <div class="shopcart">
    <div class="content" @click="toggleList">
      <div class="content-left">
        <div class="logo-wrap">
          <div class="logo" :class="{'active':totalCount>0}">
            <span class="icon-shopping_cart" :class="{'active':totalCount>0}"></span>
          </div>
          <div class="num" v-show="totalCount>0">{{totalCount}}</div>
        </div>
        <div class="price" :class="{'active':totalPrice>0}">￥{{totalPrice}}</div>
        <div class="desc">另需配送费￥{{deliveryPrice}}元</div>
      </div>
      <div class="content-right" @click.stop="pay">
        <div class="pay" :class="payClass">
          {{payDesc}}
        </div>
      </div>
    </div>
    <div class="balls-wrap">
      <transition name="drop">
        <div v-for="ball in balls" v-show="ball.show" class="ball">
          <div class="inner"></div>
        </div>
      </transition>
    </div>
    <transition name="fold">
      <div class="shopcart-list" v-show="listShow">
        <div class="list-header">
          <h1 class="title">购物车</h1>
          <span class="empty" @click="empty">清空</span>
        </div>
        <div class="list-content" ref="listContent">
          <ul>
            <li class="food" v-for="food in selectFoods">
              <span class="name">{{food.name}}</span>
              <span class="price">￥{{food.price*food.count}}</span>
              <div class="cartcontrol-wrap">
                <cartcontrol :food="food"></cartcontrol>
              </div>
            </li>
          </ul>
        </div>
      </div>
    </transition>
  </div>
  <transition name="fade">
    <div class="list-mask" v-show="listShow" @click="hideList"></div>
  </transition>
</div>
</template>

<script>
import BScroll from 'better-scroll'
import cartcontrol from 'components/cartcontrol/cartcontrol'

export default {
  name: 'shopcart',
  data: function() {
    return {
      balls: [{
        show: false
      }, {
        show: false
      }, {
        show: false
      }, {
        show: false
      }, {
        show: false
      }],
      fold: true
    }
  },
  props: {
    deliveryPrice: {
      type: Number,
      default: 0
    },
    minPrice: {
      type: Number,
      default: 0
    },
    selectFoods: {
      type: Array
    }
  },
  computed: {
    totalPrice: function() {
      let total = 0
      this.selectFoods.forEach((food) => {
        total += food.price * food.count
      })
      return total
    },
    totalCount: function() {
      let total = 0
      this.selectFoods.forEach((food) => {
        total += food.count
      })
      return total
    },
    payDesc: function() {
      if (this.totalPrice === 0) {
        return `￥${this.minPrice}元起送` //es6模板字符串
      } else if (this.totalPrice < this.minPrice) {
        let price = this.minPrice - this.totalPrice
        return `还差￥${price}元起送`
      } else {
        return '去结算'
      }
    },
    payClass: function() {
      if (this.totalPrice < this.minPrice) {
        return 'not-enough'
      } else {
        return 'enough'
      }
    },
    listShow: function() {
      if (!this.totalCount) {
        this.fold = true
        return false
      }
      let show = !this.fold
      if(show){
        this.$nextTick(()=>{
          if(!this.scroll){
            this.scroll=new BScroll(this.$refs.listContent,{click:true})
          }else{
            this.scroll.refresh()
          }
        })
      }
      return show
    }
  },
  methods: {
    toggleList: function() {
      if (!this.totalCount) {
        return
      }
      this.fold = !this.fold
    },
    empty:function(){
      this.selectFoods.forEach((food)=>{
        food.count=0
      })
    },
    hideList:function(){
      this.fold=true
    },
    pay:function(){
      if(this.totalPrice<this.minPrice){
        return
      }
      window.alert(`支付${this.totalPrice}元`)
    }
  },
  components: {
    cartcontrol
  }
}
</script>

<style lang="stylus" rel="stylesheet/stylus">
@import '../../assets/stylus/mixin.styl'
  .shopcart
    position:fixed
    left:0
    bottom:0
    z-index:50
    width:100%
    height:48px
    .content
      display:flex
      background-color:#141d27
      color: rgba(255, 255, 255, 0.4)
      .content-left
        flex-grow:1
        font-size:0
        .logo-wrap
          display:inline-block
          position:relative
          top:-10px
          margin:0 12px
          padding:6px
          width:56px
          height:56px
          box-sizing:border-box
          vertical-align:top
          border-radius:50%
          background-color:#141d27
          .logo
            width:100%
            height:100%
            border-radius:50%
            background-color:#2b343c
            text-align:center
            &.active
              background-color:rgb(0,160,220)
            .icon-shopping_cart
              font-size:24px
              color:#80858a
              line-height:44px
              &.active
                color:#fff;
          .num
            position:absolute
            top:0
            right:0
            width:24px
            height:16px
            line-height:16px
            text-align:center
            border-radius:16px
            font-size:9px
            font-weight:700
            color:#fff
            background-color:rgb(240,20,20)
            box-shadow:0 4px 8px 0 rgba(0,0,0,0.4)
        .price
          display: inline-block
          vertical-align: top
          margin-top: 12px
          line-height: 24px
          padding-right: 12px
          box-sizing: border-box
          border-right: 1px solid rgba(255, 255, 255, 0.1)
          font-size: 16px
          font-weight: 700
          &.active
            color:#fff
        .desc
          display: inline-block
          vertical-align: top
          margin: 12px 0 0 12px
          line-height: 24px
          font-size: 10px
      .content-right
        flex-basis:105px
        flex-shrink:0
        .pay
          height:48px
          line-height:48px
          text-align:center
          font-size:12px
          font-weight:700
          &.not-enough
            background-color:#2b333b
          &.enough
            background-color:#00b43c
            color:#fff
    .balls-wrap
      .ball
        position:fiexd
        left:32px
        bottom:22px
        z-index:200
        .inner
          width:16px
          height:16px
          border-radius:50%
          background-color:rgb(0,160,220)
          transition:all 0.4s
        &.drop-enter-active
          transition:all 0.4s
    .shopcart-list
      position:absolute
      left:0
      top:0
      z-index:-1
      width:100%
      transform:translate3d(0,-100%,0)
      &.fold-enter-active,&.fold-leave-active
        transition:all 0.5s
      &.fold-enter,&.fold-leave-active
        transform:translate3d(0,0,0)
      .list-header
        height:40px
        line-height:40px
        padding:0 18px
        background-color:#f3f5f7
        border-bottom:1px solid rgba(7,17,27,0.1)
        .title
          float: left
          font-size: 14px
          color: rgb(7, 17, 27)
        .empty
          float: right
          font-size: 12px
          color: rgb(0, 160, 220)
      .list-content
        padding:0 18px
        max-height:217px
        background-color:#fff
        overflow:hidden
        .food
          position:relative
          padding:12px 0
          box-sizing:border-box
          border-1px(rgba(7,17,27,0.1))
          .name
            line-height:24px
            font-size:14px
            color: rgb(7, 17, 27)
          .price
            position: absolute
            right: 90px
            bottom: 12px
            line-height: 24px
            font-size: 14px
            font-weight: 700
            color: rgb(240, 20, 20)
          .cartcontrol-wrap
            position: absolute
            right: 0
            bottom: 6px
  .list-mask
    position:fixed
    top:0
    left:0
    right:0
    bottom:0
    z-index:40
    background-color:rgba(7,17,27,0.6)
    backdrop-filter:blur(10px)
    opacity:1
    &.fade-enter-active,&.fade-leave-active
      transition:all 0.5s
    &.fade-enter,&.fade-leave-active
      opacity:0
</style>
