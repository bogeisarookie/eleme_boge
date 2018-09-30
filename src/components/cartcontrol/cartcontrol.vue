<template>
	<div class="cartcontrol">
		<transition name="move">
			<div class="cart-decrease" @click.stop.prevent="decreaseCart($event)" v-show="food.count>0">
				<span class="inner icon-remove_circle_outline"></span>
			</div>
		</transition>

		<div class="cart-count" v-show="food.count>0">{{food.count}}</div>
		<div class="cart-add icon-add_circle" @click.stop.prevent="addCart($event)"></div>
	</div>
</template>

<script>
import Vue from "vue";
export default {
  props: {
    food: {
      type: Object
    }
  },
  created() {},
  methods: {
    addCart(event) {
      if (!event._constructed) {
        return;
      }
      if (!this.food.count) {
        // this.food.count = 1; 这样自动给原数据增加新的属性并赋值，Vue是检测不到变化的，所以DOM也不会发生变化。需要用set方法
        Vue.set(this.food, "count", 1);
      } else {
        this.food.count++;
      }
			this.$emit('add',event.target)
    },
    decreaseCart(event) {
      if (!event._constructed) {
        return;
      }
      if (this.food.count >= 0) {
        this.food.count--;
      }
    }
  }
};
</script>
 
 <style lang="stylus" scoped>
 .cartcontrol
 	font-size 0 // 为了让inline-block之间没有间隙
 	.cart-decrease
 		display inline-block // 横向排列
 		padding 6px // 通过padding将点击范围变大
 		opacity 1
 		transform translate3d(0, 0, 0) // 开启硬件加速让动画更流畅
 		.inner
 			display inline-block // 这样才有宽高
 			line-height 24px
 			font-size 24px
 			color rgb(0, 160, 220)
 			transition all 0.4s linear
 			transform rotate(0)
 		&.move-enter-active, &.move-leave-active
 			transition all 0.4s linear
 		&.move-enter, &.move-leave-active
 			opacity 0
 			transform translate3d(24px, 0, 0)
 			.inner
 				transform rotate(180deg)
 	.cart-count
 		display inline-block
 		vertical-align top
 		width 12px
 		padding-top 6px
 		line-height 24px
 		text-align center
 		font-size 10px
 		color rgb(147, 153, 159)
 	.cart-add
 		display inline-block
 		padding 6px
 		line-height 24px
 		font-size 24px
 		color rgb(0, 160, 220)
</style>
 
