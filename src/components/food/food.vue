<template>
  <transition name="move">
    <div v-show="showFlag" class="food">
      <div class="image-header">
        <img :src="food.image" >
      </div>
    </div>
  </transition>
</template>

<script>
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
          console.log(this.food);
      }
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
    &.move-enter,&.move-leave-active
      transform translate3d(100%, 100%, 0)
</style>
