<template>
  <div class="cartcontrol">
    <transition name="move">
      <div class="cart-decrease icon-remove_circle_outline" v-show="food.count>0" @click="decreaseCart"></div>
    </transition>
    <div class="cart-count" v-show="food.count>0">{{food.count}}</div>
    <div class="cart-add icon-add_circle" @click="addCart"></div>
  </div>
</template>

<script type="text/ecmascript-6">
  import Vue from 'vue';
  export default {
    props: {
      food: {
        type: Object
      }
    },
    methods: {
      addCart(event) {
        if (!event._constructed) {
          return;
        }
        if (!this.food.count) {
          Vue.set(this.food, 'count', 1);
        } else {
          this.food.count++;
        }
      },
      decreaseCart(event) {
        if (!event._constructed) {
          return;
        }
        if (this.food.count <= 0) {
          return;
        }
        this.food.count--;
      }
    }
  }
  ;
</script>
<style lang="stylus" rel="stylesheet/stylus">
  .cartcontrol
    font-size 0
    .move-enter-active
      animation: bounce-in .5s;
    .move-leave-active
      animation: bounce-in .5s reverse;
    //  transition: all .8s cubic-bezier(1.0, 0.5, 0.8, 1.0);
   // .move-enter,.move-leave
   //   transform: translateX(60px);
   //   transform: rotate(180deg);
   //   opacity 0
    .cart-add, .cart-decrease
      display inline-block
      padding 6px
      font-size 24px
      line-height 24px
      color rgb(0, 160, 220)
    .cart-count
      display inline-block
      font-size 10px
      vertical-align top
      width 12px
      padding-top 6px
      line-height 24px
      text-align center
      color rgb(147, 153, 159)
    .cart-add
      display inline-block
  @keyframes bounce-in {
    0% {
      opacity 0;
      translateX(0);
      transform: rotate(0deg);
    }
    50%{
      opacity 0.4;
      transform: rotate(100deg) translateX(40deg);
    }
    100% {
      opacity 1;
      translateX(60px);
      transform: rotate(180deg);
    }
  }
</style>
