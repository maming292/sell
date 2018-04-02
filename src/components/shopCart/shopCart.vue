<template>
  <div class="shopcart">
    <div class="content">
      <div class="content-left" @click="toggleList">
        <div class="logo-wrapper">
          <div class="logo" :class="{'highlight':totalCount > 0}">
            <i class="icon-shopping_cart" :class="{'highlight':totalCount > 0}"></i>
          </div>
          <div class="num" v-show="totalCount>0">{{totalCount}}</div>
        </div>
        <div class="price" :class="{'highlight':totalPrice > 0}">￥{{totalPrice}}</div>
        <div class="desc">另需配送费{{deliveryprice}}￥</div>
      </div>
      <div class="content-right" @click="pay">
        <div class="pay" :class="payClass">
          {{payDesc}}
        </div>
      </div>
    </div>
    <div class="ball-container">
      <template v-for="(ball,id) in balls">
        <transition name="drop"  @before-enter = 'beforeEnter' @enter = 'enter'  @after-enter = 'afterEnter' :key="id">
          <div v-show="ball.show" class="ball">
            <div class="inner inner-hook"></div>
          </div>
        </transition>
      </template>
    </div>
    <transition name="fade">
      <div class="list-mask" v-show="listShow" @click="hideList"></div>
    </transition>
    <transition name="fold">
      <div class="shopcart-list" v-show="listShow">
      <div class="list-header">
        <h1 class="title">购物车</h1>
        <span class="empty" @click="empty">清空</span>
      </div>
      <div class="list-content" ref="listcontent">
        <ul>
          <li class="food" v-for="(food,id) in selectFoods" :key="id">
            <span class="name">{{food.name}}</span>
            <div class="price">
              <span>￥{{food.price*food.count}}</span>
            </div>
            <div class="cartcontrol-wrapper">
              <cartcontrol :food="food"></cartcontrol>
            </div>
          </li>
        </ul>
      </div>
    </div>
    </transition>

  </div>
</template>
<script type="text/ecmascript-6">
  import BScroll from 'better-scroll';
  import cartcontrol from '../cartControl/cartControl.vue';
  export default {
    props: {
      deliveryprice: {
        type: Number,
        defaulr: 0
      },
      minprice: {
        type: Number,
        defaulr: 0
      },
      selectFoods: {
        typr: Array,
        default() {
          return [
            {
              price: 0,
              count: 0
            }
          ];
        }
      }
    },
    data() {
      return {
        balls: [
          {show: false},
          {show: false},
          {show: false},
          {show: false},
          {show: false}
        ],
        dropBalls: [],
        fold: true
      };
    },
    computed: {
      listShow() {
        if (!this.totalCount) {
          this.fold = true;
          return false;
        }
        let show = !this.fold;
        if (show) {
          this.$nextTick(() => {
            if (!this.scroll) {
              this.scroll = new BScroll(this.$refs.listcontent, {
                click: true
              });
            } else {
              this.scroll.refresh();
            }
          });
        }
        return show;
      },
      totalPrice() {
        let total = 0;
        this.selectFoods.forEach((food) => {
          total += food.price * food.count;
        });
        return total;
      },
      totalCount() {
        let count = 0;
        this.selectFoods.forEach((food) => {
          count += food.count;
        });
        return count;
      },
      payDesc() {
        if (this.totalPrice === 0) {
          return `￥${this.minprice}元起送`;
        } else if (this.totalPrice < this.minprice) {
          let diff = this.minprice - this.totalPrice;
          return `还差￥${diff}元起送`;
        } else {
          return '去结算';
        }
      },
      payClass() {
        if (this.totalPrice < this.minprice) {
          return 'no-enough';
        } else {
          return 'enough';
        }
      }
    },
    methods: {
      pay() {
        if (this.totalPrice < this.minprice) {
          return;
        }
        window.alert(`支付${this.totalPrice}元`);
      },
      toggleList() {
        if (!this.totalCount) {
          return;
        }
        this.fold = !this.fold;
      },
      hideList() {
        this.fold = !this.fold;
      },
      drop(el) {
//        console.log(el);
        for (let i = 0; i < this.balls.length; i++) {
          let ball = this.balls[i];
          if (!ball.show) {
            ball.show = true;
            ball.el = el;
            this.dropBalls.push(ball);
            return;
          }
        }
      },
      beforeEnter(el) {
        let count = this.balls.length;
        while (count--) {
          let ball = this.balls[count];
          if (ball.show) {
            //  getBoundingClientRect()  返回 left top right bottom
            let rect = ball.el.getBoundingClientRect();
            let x = rect.left - 32;
            let y = -(window.innerHeight - rect.top - 22);
            el.style.display = '';
            el.style.webkitTtansform = `translate3d(0,${y}px,0)`;
            el.style.transform = `translate3d(0,${y}px,0)`;
            let inner = el.getElementsByClassName('inner-hook')[0];
            inner.style.webkitTtansform = `translate3d(${x}px,0,0)`;
            inner.style.transform = `translate3d(${x}px,0,0)`;
          }
        }
      },
      enter(el) {
        /* eslint-disable no-unused-vars */
         let rf = el.offsetHeight;
        this.$nextTick(() => {
          el.style.webkitTtansform = 'translate3d(0,0,0)';
          el.style.transform = 'translate3d(0,0,0)';
          let inner = el.getElementsByClassName('inner-hook')[0];
          inner.style.webkitTtansform = 'translate3d(0,0,0)';
          inner.style.transform = 'translate3d(0,0,0)';
        });
      },
      afterEnter(el) {
        let ball = this.dropBalls.shift();
        if (ball) {
          ball.show = false;
          el.style.display = 'none';
        }
      },
      empty() {
        this.selectFoods.forEach((food) => {
          food.count = 0;
        });
      }
    },
    components: {
      cartcontrol
    }
  };
</script>
<style lang="stylus" rel="stylesheet/stylus">
  @import "../../common/stylus/mixin.styl"
  .shopcart
    position fixed
    bottom: 0
    left 0
    z-index 50
    height 48px
    width: 100%
    /*background #000*/
    .content
      display flex
      background #141d27
      height: 100%
      font-size 0
      .content-left
        flex 1
        position relative
        .logo-wrapper
          box-sizing border-box
          display inline-block
          position relative
          top -10px
          margin 0 12px
          padding: 6px
          width 56px
          height 56px
          background #141d27
          border-radius 50%
          .logo
            background #2b343c
            border-radius 50%
            width 100%
            text-align center
            &.highlight
              background rgb(0, 160, 220)
            .icon-shopping_cart
              display inline-block
              color: rgba(255, 255, 255, 0.4)
              font-size 24px
              height 100%
              line-height 44px
              &.highlight
                color: #fff
          .num
            position absolute
            top: 0
            right: 0
            width: 24px
            height 16px
            line-height 16px
            text-align center
            border-radius 8px
            font-size 9px
            font-weight 700
            color: #fff
            background rgb(240, 20, 20)
            box-shadow 0 4px 8px 0 rgba(0, 0, 0, 0.4)
        .price
          display inline-block
          vertical-align top
          line-height 24px
          margin-top 12px
          box-sizing border-box
          padding-right 12px
          border-right 1px solid rgba(255, 255, 255, 0.1)
          font-size 16px
          font-weight 700
          color rgba(255, 255, 255, 0.4)
          &.highlight
            color: #fff
        .desc
          display inline-block
          vertical-align top
          line-height 24px
          margin 12px 0 0 12px
          font-size 10px
          color rgba(255, 255, 255, 0.4)
      .content-right
        flex 0 0 105px
        width: 105px
        background #2b333b
        .pay
          height: 48px
          line-height 48px
          text-align center
          font-size 12px
          color rgba(255, 255, 255, 0.4)
          font-weight 700;
          &.no-enough
            background #2b333b
          &.enough
            background #00b43c
            color #fff
    .ball-container
      .ball
        position fixed
        left 32px
        bottom 22px
        z-index: 200
        transition: all 0.8s cubic-bezier(.49, -0.29, .75, .41)
        .inner
          width 16px
          height 16px
          border-radius 50%
          background rgb(0, 160, 220)
          transition: all 0.8s linear
    .shopcart-list
      position absolute
      top: 0
      left 0
      z-index -1
      width 100%
      transition all 0.5s
      transform translate3d(0,-100%,0)
      background rgba(7,17,27,0.3)
      &.fold-enter,&.fold-leave-active
        transform translate3d(0,0,0)
      .list-header
        height 40px
        line-height 40px
        padding 0 18px
        background #f3f5f7
        border-bottom 1px solid rgba(7,17,27,0.1)
        .title
          float: left
          font-size 14px
          color rgb(7,17,27)
        .empty
          float right
          font-size 12px
          color rgb(0,160,220)
      .list-content
        padding 0 18px
        max-height 217px
        background #fff
        overflow hidden
        .food
          position relative
          padding 12px
          box-sizing border-box
          border-1px(rgba(7,17,27,0.1))
          .name
            line-height: 24px
            font-size 14px
            color: rgb(7,17,27)
          .price
            position absolute
            right 90px
            bottom 12px
            font-size: 14px
            line-height 24px
            font-weight 700
            color rgb(240,20,20)
          .cartcontrol-wrapper
            position absolute
            right:6px
            bottom 6px
    .list-mask
      position fixed
      top:0
      left:0
      width 100%
      height: 100%
      z-index -5
      backdrop-filter blur(10px)
      background rgba(7,17,27,0.6)
      transition all 0.5s
      &.fade-enter,&.fade-leave-active
        background rgba(7,17,27,0)
</style>
