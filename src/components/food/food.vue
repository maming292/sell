<template>
  <transition name="move">
      <div v-show="showFlag" class="food" ref="food">
        <div class="food-content">
        <div class="image-header">
          <img :src="food.image">
          <div class="back" @click="back">
            <i class="icon-arrow_lift"></i>
          </div>
        </div>
        <div class="content">
          <h1 class="title">{{food.name}}</h1>
          <div class="detail">
            <span class="sell-count">月售{{food.sellCount}}份</span>
            <span class="rating">好评率{{food.rating}}%</span>
          </div>
          <div class="price">
            <span class="now">￥{{food.price}}</span>
            <span class="old" v-show="food.oldPrice">￥{{food.oldPrice}}</span>
          </div>
        </div>
        <div class="cartcontrol-wrapper" v-show="food.count">
          <cartcontrol :food="food"></cartcontrol>
        </div>
        <div @click="addFirst($event)" class="buy" v-show="!food.count || food.count ===0">加入购物车</div>
        </div>
      </div>
  </transition>
</template>

<script>
  import Bsroll from 'better-scroll';
  import cartcontrol from '../cartControl/cartControl.vue';
  import Vue from 'vue';
  export default {
    props: {
      food: {
        type: Object
      }
    },
    data() {
      return {
        showFlag: false
      };
    },
    methods: {
      show() {
        this.showFlag = true;
        this.$nextTick(() => {
          if (!this.scroll) {
            this.scroll = new Bsroll(this.$refs.food, {
              click: true
            });
          } else {
            this.scroll.refresh();
          }
        });
      },
      back() {
        this.showFlag = false;
      },
      addFirst(event) {
        if (!event._constructed) {
          return;
        }
        Vue.set(this.food, 'count', 1);
      }
    },
    components: {
      cartcontrol
    }
  };
</script>
<style lang="stylus" rel="stylesheet/stylus">
  .food
    position: fixed
    left: 0
    top: 0
    bottom: 48px
    z-index: 30
    width: 100%
    background: #ffffff
    transition: all 0.2s linear
    &.move-enter
      transform translate3d(100%, 0, 0)
    &.move-leave-active
      transform translate3d(-100%, 0, 0)
    .image-header
      position relative
      width 100%
      height 0
      padding-top 100%
      img
        position absolute
        left 0
        top 0
        width 100%
        height 100%
      .back
        position absolute
        top 10px
        left 0
        .icon-arrow_lift
          display block
          padding 10px
          font-size 20px
          color #ffffff
    .content
      padding 18px
      .title
        line-height 14px
        margin-bottom 8px
        font-size 14px
        font-weight 700
        color rgb(7, 17, 27)
      .detail
        margin-bottom 18px
        line-height 10px
        height 10px
        font-size 0
        .sell-count, .rating
          font-size 10px
          color rgb(147, 153, 159)
        .sell-count
          margin-right 12px
      .price
        font-weight: 700
        line-height 24px
        .now
          margin-right 8px
          font-size 14px
          color: rgb(240, 20, 20)
        .old
          text-decoration line-through
          font-size 10px
          color: rgb(147, 153, 159)
    .cartcontrol-wrapper
      position absolute
      right 12px
      bottom: 12px
    .buy
      position absolute
      right:18px
      bottom:18px
      z-index 10
      height 24px
      line-height 24px
      padding 0 12px
      box-sizing border-box
      border-radius 12px
      font-size 10px
      color #fff
      background rgba(0,160,220,1)
</style>