<template>
  <div class="headers">
    <div class="content-wapper">
      <div class="avatar">
        <img :src="seller.avatar" width="64">
      </div>
      <div class="content">
        <div class="title">
          <span class="brand">
            <img src="">
          </span>
          <span class="name">{{seller.name}}</span>
        </div>
        <div class="description">
          {{seller.description}}/{{seller.deliveryTime}}分钟送达
        </div>
        <div v-if="seller.supports" class="support">
          <span class="icon" :class="classMap[seller.supports[0].type]"></span>
          <span class="text">{{seller.supports[0].description}}</span>
        </div>
      </div>
      <div v-if="seller.supports" class="support-count">
        <span class="count">{{seller.supports.length}}</span>
        <i class="icon-thumb_up"></i>
      </div>
    </div>
    <div class="bulletin-wrapper"></div>
  </div>
</template>

<script>
  export default {
      props: {
        seller: {
          type: Object
        }
      },
      created() {
        this.classMap = ['decrease', 'discount', 'guarantee', 'invoice', 'special'];
      }
  };
</script>

<style lang="stylus" rel="stylesheet/stylus">
  @import "../../common/stylus/mixin.styl"
  .headers
    color: #fff
    background #000
    .content-wapper
      position relative
      padding 24px 12px 18px 24px
      font-size 0
      .avatar
        display inline-block
        vertical-align top
        img
         border-radius 2px
      .content
        display inline-block
        margin-left 16px
        .title
          margin 2px 0 8px 0
          .brand
            display: inline-block
            vertical-align top
            width: 30px
            height: 18px
            bg-image("brand")
            background-size :30px 18px
            background-repeat :no-repeat
          .name
             margin-left 6px
             font-size 16px
             line-height 18px
             font-weight:700
        .description
          margin-bottom 10px
          line-height 12px
          font-size 12px
        .support
          .icon
            display inline-block
            vertical-align top
            height: 12px
            width: 12px
            margin-right 4px
            background-size 12px 12px
            background-repeat no-repeat
            &.decrease
              bg-image('decrease_1')
            &.discount
              bg-image('discount_1')
            &.guarantee
               bg-image('guarantee_1')
            &.invoice
              bg-image('invoice_1')
            &.special
              bg-image('special_1')
          .text
            display inline-block
            font-size 10px
            line-height 12px

      .support-count
        position absolute
        right 12px
        bottom 18px
        padding: 0 8px
        height: 24px
        line-height 12px
        border-radius 14px
        background rgba(0,0,0,0.2)
        text-align center
        .count
          font-size 10px
          .icon-thumb_up
              font-size 10px
</style>
