<template>
  <div class="rating"></div>
</template>

<script>
  const POSITIVE = 0;
  const NEGATIVE = 1;
  const ALL = 2;
  export default {
    props: {
      ratings: {
        type: Array,
        default() {
          return [];
        }
      }
    }
  };
</script>

<style lang="stylus" rel="stylesheet/stylus">
</style>
