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
            <span class="now">￥${{food.price}}</span>
            <span class="old" v-show="food.oldPrice">￥{{food.oldPrice}}</span>
          </div>
          <div class="cartcontrol-wrapper" v-show="food.count">
            <cartcontrol :food="food" @adds="drop"></cartcontrol>
          </div>
          <div @click="addFirst($event)" class="buy" v-show="!food.count || food.count ===0">加入购物车</div>
        </div>
        <split v-show="food.info"></split>
        <div class="info" v-show="food.info">
          <h1 class="title">商品信息</h1>
          <p class="text">{{food.info}}</p>
        </div>
        <split></split>
        <div class="rating">
          <h1 class="title">商品评价</h1>
          <ratingselect :select-type="selectType" :only-content="onlyContent" :desc="desc" :ratings="food.ratings"
                        @changeType="changeType" @toggleContent="toggleContent"></ratingselect>
          <div class="rating-wrapper">
            <ul v-show="food.ratings && food.ratings.length">
              <li v-show="needshow(ratings.rateType,ratings.text)" v-for="(ratings,id) in food.ratings" :key="id"
                  class="rating-item">
                <div class="user">
                  <span class="name">{{ratings.username}}</span>
                  <img class="avatar" width="12" height="12" :src="ratings.avatar">
                </div>
                <div class="time">{{ratings.rateTime | formatDate}}</div>
                <p class="text">
                  <span
                    :class="{'icon-thumb_up':ratings.rateType===0,'icon-thumb_down':ratings.rateType===1}"></span>{{ratings.text}}
                </p>
              </li>
            </ul>
            <div class="no-rating" v-show="!food.ratings || !food.ratings.length"><span>暂无评价</span></div>
          </div>
        </div>
      </div>
    </div>
  </transition>
</template>
<script>
  import Bsroll from 'better-scroll';
  import cartcontrol from '../cartControl/cartControl';
  import Vue from 'vue';
  import split from '../split/split';
  import ratingselect from '../ratingSelect/ratingSelect';
  import {formatDate} from '../../common/js/date';
  const ALL = 2;

  export default {
    props: {
      food: {
        type: Object
      }
    },
    data() {
      return {
        showFlag: false,
        selectType: ALL,
        onlyContent: true,
        desc: {
          all: '全部',
          positive: '推荐',
          negative: '吐槽'
        }
      };
    },
    methods: {
      needshow(type, text) {
        if (this.onlyContent && !text) {
          return false;
        }
        if (this.selectType === ALL) {
          return true;
        } else {
          return type === this.selectType;
        }
      },
      changeType(e) {
        this.selectType = e;
      },
      toggleContent(content) {
        this.onlyContent = content;
      },
      drop(e) {
        this.$emit('adds', e);
      },
      show() {
        this.showFlag = true;
        this.selectType = ALL;
        this.onlyContent = false;
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
        this.$emit('adds', event);
        Vue.set(this.food, 'count', 1);
      }
    },
    filters: {
      formatDate(time) {
        let date = new Date(time);
        return formatDate(date, 'yyyy-MM-dd hh:mm');
      }
    },
    components: {
      cartcontrol,
      split,
      ratingselect
    }
  };
</script>
<style lang="stylus" rel="stylesheet/stylus">
  @import "../../common/stylus/mixin.styl"
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
      position relative
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
        right: 18px
        bottom: 18px
        z-index 10
        height 24px
        line-height 24px
        padding 0 12px
        box-sizing border-box
        border-radius 12px
        font-size 10px
        color #fff
        background rgba(0, 160, 220, 1)
    .info
      padding: 18px
      .title
        line-height 14px
        margin-bottom: 6px
        font-size 14px
        color rgb(7, 17, 27)
      .text
        line-height 24px
        font-size 12px
        color rgb(77, 85, 93)
        padding 0 8px
    .rating
      padding-top 18px
      .title
        line-height 14px
        margin-left 18px
        font-size 14px
        color rgb(7, 17, 27)
      .rating-wrapper
        padding: 0 18px
        .rating-item
          position relative
          padding: 16px 0
          border-1px(rgba(7, 17, 27, 0.1))
          .user
            position absolute
            right: 0
            top: 16px
            line-height 12px
            font-size 0
            .name
              display inline-block
              vertical-align top
              margin-right 6px
              font-size 10px
              color rgb(147, 153, 159)
            .avatar
              border-radius 50%

          .time
            margin-bottom 6px
            line-height 12px
            font-size 10px
            color rgb(147, 153, 159)
          .text
            line-height 16px
            font-size 12px
            color rgb(7, 17, 27)
            .icon-thumb_up, .icon-thumb_down
              line-height 16px
              margin-right 4px
              font-size 12px
            .icon-thumb_up
              color: rgb(6, 160, 220)
            .icon-thumb_down
              color rgb(147, 153, 159)
        .no-rating
          padding: 16px 0
          font-size 12px
          color rgb(147,153,159)
</style>
