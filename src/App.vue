<template>
  <div id="app">
    <vheader :seller="seller"></vheader>
    <div class="tab">
      <div class="tab-item">
        <router-link class="as" to="/goods">商品</router-link>
      </div>
      <div class="tab-item">
        <router-link class="as" to="/ratings">评价</router-link>
      </div>
      <div class="tab-item">
        <router-link class="as" to="/seller">商家</router-link>
      </div>
    </div>
    <keep-alive>
      <router-view :seller="seller">
      </router-view>
    </keep-alive>
  </div>
</template>
<script type="text/ecmascript-6">
  import headers from './components/header/header.vue';
  import {urlParse} from './common/js/util';

  // const ERR_OK = 0;
  export default {
    data() {
      return {
        seller: {
          id: (() => {
            let queryParam = urlParse();
            return queryParam.id;
          })()
        }
      };
    },
    created() {
      this.$http.jsonp('http://192.168.3.2/dist/data.php?id=' + this.seller.id).then((response) => {
        response = response.body.seller;
        if (response) {
          // console.log(response);
          this.seller = Object.assign({}, this.seller, response);
        }
      }).catch((e) => {
        console.log(e);
      });
    },
    components: {
      vheader: headers
    }
  };
</script>
<style lang="stylus" rel="stylesheet/stylus">
  @import "common/stylus/mixin.styl";
  #app
    .tab
      display: flex
      width: 100%
      height: 40px
      line-height: 40px
      border-1px(rgba(7, 17, 27, 0.1))
      .tab-item
        flex: 1
        text-align center
        & > a
          display: block
          font-size: 14px
          color: rgb(77, 85, 93)
          &.active
            color: rgb(240, 20, 20)
</style>
