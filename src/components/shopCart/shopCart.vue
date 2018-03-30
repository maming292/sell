<template>
  <div class="shopcart">
    <div class="content">
      <div class="content-left">
        <div class="logo-wrapper">
          <div class="logo" :class="{'highlight':totalCount > 0}">
            <i class="icon-shopping_cart" :class="{'highlight':totalCount > 0}"></i>
          </div>
          <div class="num" v-show="totalCount>0">{{totalCount}}</div>
        </div>
        <div class="price" :class="{'highlight':totalPrice > 0}">￥{{totalPrice}}</div>
        <div class="desc">另需配送费{{deliveryprice}}￥</div>
      </div>
      <div class="content-right">
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
  </div>
</template>
<script type="text/ecmascript-6">
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
        dropBalls: []
      };
    },
    computed: {
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
            // console.log(el.style.transform);
            // console.log(inner.style.transform);
          }
        }
      },
      enter(el) {
        // let rf = el.offsetHeight;
        this.$nextTick(() => {
          el.style.webkitTtansform = 'translate3d(0,0,0)';
          el.style.transform = 'translate3d(0,0,0)';
          let inner = el.getElementsByClassName('inner-hook')[0];
          inner.style.webkitTtansform = 'translate3d(0,0,0)';
          inner.style.transform = 'translate3d(0,0,0)';
          console.log(el.style.transform);
          console.log(inner.style.transform);
        });
      },
      afterEnter(el) {
        let ball = this.dropBalls.shift();
        if (ball) {
          ball.show = false;
          el.style.display = 'none';
        }
      }
    }
  };
</script>
<style lang="stylus" rel="stylesheet/stylus">
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
    background #f00
    transition: all 5.4s cubic-bezier(.49, -0.29, .75, .41)
    .inner
      width 16px
      height 16px
      border-radius 50%
      background rgb(0, 160, 220)
      transition: all 5.4s linear
</style>
