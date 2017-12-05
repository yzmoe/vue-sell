<template lang="html">
<div class="cartcontrol">
  <transition name="move">
    <span class="cart-decrease" v-show="food.count>0" @click.stop="decreaseCart">
      <span class="inner icon-remove_circle_outline"></span>
    </span>
  </transition>
  <span class="cart-count" v-show="food.count>0">{{food.count}}</span>
  <span class="cart-add icon-add_circle" @click.stop="addCart">
  </span>
</div>
</template>

<script>
import Vue from 'vue'
export default {
  name:'cartcontrol',
  props:{
    food:{
      type:Object,
    }
  },
  created:function(){
    // console.log(this.food)
  },
  methods:{
    addCart:function(event){
      if (!event._constructed) {
        return
      }
      if(!this.food.count){
        // this.food.count=1
        // console.log(this.food.count) //Vue 不能检测属性被添加
        Vue.set(this.food,'count',1)     //Vue中添加属性应用Vue.set()
      }else{
        this.food.count++
        // console.log(this.food.count) //Vue 不能检测属性被添加
      }
    },
    decreaseCart:function(event){
      if (!event._constructed) {
        return
      }
      this.food.count--
    }
  }
}
</script>

<style lang="stylus" rel="stylesheet/stylus">
  .cartcontrol
    font-size:0
    .cart-add
      display:inline-block
      padding:6px
      font-size:24px
      line-height:24px
      color:rgb(0,160,220)
    .cart-count
      display:inline-block
      vertical-align:top
      width:12px
      padding-top:6px
      line-height:24px
      text-align:center
      font-size:10px
      color:rgb(147,153,159)
    .cart-decrease
      display:inline-block
      padding:6px
      opacity:1
      transform: translate3d(0, 0, 0)
      .inner
        font-size:24px
        line-height:24px
        color:rgb(0,160,220)
        transform: rotate(0)
        transition: all 0.4s linear
      &.move-enter-active, &.move-leave-active
        transition: all 0.4s linear
      &.move-enter, &.move-leave-active
        opacity:0
        transform: translate3d(24px, 0, 0)
        .inner
          transform: rotate(180)
</style>
