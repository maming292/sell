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
    <router-view :seller="seller">
    </router-view>
  </div>
</template>
<script type="text/ecmascript-6">
  import headers from './components/header/header.vue';
  const ERR_OK = 0;
  export default {
    data() {
      return {
        seller: {}
      };
    },
    created() {
      this.$http.get('/api/seller').then((response) => {
        response = response.body;
        // console.log(response.data);
        if (response !== ERR_OK) {
          this.seller = response.data;
        }
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
      width:100%
      height: 40px
      line-height: 40px
      border-1px(rgba(7,17,27,0.1))
      .tab-item
          flex :1
          text-align center
          & > a
            display:block
            font-size: 14px
            color: rgb(77,85,93)
            &.active
             color: rgb(240,20,20)
</style>
