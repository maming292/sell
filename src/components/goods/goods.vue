<template>
  <div class="goods">
    <div class="menu-wrapper">
      <ul>
        <li v-for="(item,id) in goods" :key="id" class="menu-item">
          <span class="text">
           <span v-show="item.type > 0" class="icon" :class="classMap[item.type]"></span> {{item.name}}
          </span>
        </li>
      </ul>
    </div>
    <div class="footer-wrapper"></div>
  </div>
</template>
<script type="text/ecmascript-6">
  const ERR_OK = 0;
  export default {
    props: {
      seller: {
        type: Object
      }
    },
    data() {
     return {
       goods: []
     };
    },
    created() {
      this.classMap = ['decrease', 'discount', 'guarantee', 'invoice', 'special'];
      //              [减，折扣，担保，发票，特价]
      this.$http.get('/api/goods').then((response) => {
        response = response.body;
        if (response.errno === ERR_OK) {
          this.goods = response.data;
          // console.log(this.goods);
        }
      });
    }
  };
</script>
<style lang="stylus" rel="stylesheet/stylus">
  @import "../../common/stylus/mixin.styl"
  .goods
    position absolute
    display: flex
    width 100%
    top 174px
    bottom 46px
    overflow hidden
    .menu-wrapper
      flex 0 0 80px
      width: 80px
      background #f3f5f7
      overflow auto
      overflow-y: hidden
      .menu-item
        display table
        height:54px
        width:56px
        line-height 14px
        .icon
          display inline-block
          vertical-align top
          height: 12px
          width: 12px
          margin-right 4px
          background-size 12px 12px
          background-repeat no-repeat
          &.decrease
            bg-image('decrease_3')
          &.discount
            bg-image('discount_3')
          &.guarantee
            bg-image('guarantee_3')
          &.invoice
            bg-image('invoice_3')
          &.special
            bg-image('special_3')

        .text
          display table-cell
          width:56px
          vertical-align middle
          border-1px(rgba(1,17,27,0.1))
          font-size 12px
    .footer-wrapper
      flex 1
</style>
