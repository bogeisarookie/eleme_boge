<template>
  <div v-show="showFlag" class="food">

  </div>
</template>

<script>
export default {
  props:{
      food:{
          type:Object
      }
  }
}
</script>

<style lang="stylus" scoped>
.food
    position fixed//相对于屏幕
    left 0
    top 0
    bottom 48px//底部有购物车
    z-index 30
    width 100%
    background #fff
    
</style>
