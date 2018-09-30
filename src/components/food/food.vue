<template>
	<transition name="move">
		<div v-show="showFlag" class="food" ref="food">
			<div class="food-content">
				<div class="image-header">
					<img :src="food.image">
					<div class="back" @click="hide">
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
						<span class="now">￥{{food.price}}</span>
						<span class="old" v-show="food.oldPrice">￥{{food.oldPrice}}</span>
					</div>
					<div class="cartcontrol-wrapper">
						<cartcontrol @add="addFood" :food="food"></cartcontrol>
					</div>
					<transition name="fade">
						<div class="buy" v-show="!food.count||food.count===0" @click.stop.prevent="addFirst">加入购物车</div>
					</transition>
				</div>
				<split v-show="food.info"></split>
				<div class="info" v-show="food.info">
					<h1 class="title">商品信息</h1>
					<p class="text">{{food.info}}</p>
				</div>
				<split></split>
				<div class="rating">
					<h1 class="title">商品评价</h1>
					<ratingselect :select-type="selectType" :only-content="
					onlyContent" :desc="desc" :ratings="food.ratings"></ratingselect>
				</div>
			</div>
		</div>
	</transition>

</template>

<script>
import BScroll from "better-scroll";
import Vue from "Vue";
import cartcontrol from "components/cartcontrol/cartcontrol.vue";
import split from "components/split/split";
import ratingselect from 'components/ratingselect/ratingselect'

const POSITIVE=0
const NEGATIVE=1
const ALL=2

export default {
  props: {
    food: {
      type: Object
    }
  },
  data() {
    return {
      showFlag: false,
			selectType:ALL,
			onlyContent:true,
			desc:{
				all:'全部',
				positive:'推荐',
				negative:'吐槽'
			}
    }
  },
  methods: {
    /**
      方法命名规则
      名字前面不加下划线代表为公有方法，父组件可以调用
      加下划线代表为私有方法，自己私用
       */
    show() {
      this.showFlag = true;
			this.selectType=ALL;
			this.onlyContent=true;
      //保证dom被渲染出来从而正确计算高度
      this.$nextTick(() => {
        if (!this.scroll) {
          this.scroll = new BScroll(this.$refs.food, {
            click: true
          });
        } else {
          this.scroll.refresh();
        }
      });
    },
    hide() {
      this.showFlag = false;
    },
    addFirst(event) {
      if (!event._constructed) {
        return;
      }
      // this.food.count = 1; 这样自动给原数据增加新的属性并赋值，Vue是检测不到变化的，所以DOM也不会发生变化。需要用set方法
      //从点击事件那里传过来的，所以是event.target
      this.$emit("add", event.target);
      Vue.set(this.food, "count", 1);
    },
    addFood(target) {
      //从cartcontrol子组件传过来的，所以直接获取的target
      this.$emit("add", target);
    }
  },
  components: {
    cartcontrol,
    split,
		ratingselect
  }
};
</script>

<style lang="stylus" scoped>
.food
	position fixed // 相对于屏幕
	left 0
	top 0
	bottom 48px // 底部有购物车
	z-index 30
	width 100%
	background #fff
	transform translate3d(0, 0, 0)
	&.move-enter-active, &.move-leave-active
		transition all 0.2s linear
	&.move-enter, &.move-leave-to
		transform translate3d(100%, 0, 0)
	.image-header
		position relative
		width 100%
		height 0
		padding-top 100% // 宽高相等的容器
		img
			position absolute // 相对于距该元素最近的已定位的祖先元素进行定位
			top 0
			left 0
			width 100%
			height 100%
		.back
			position absolute
			top 10px
			left 0
			.icon-arrow_lift
				display block // 因为要加padding
				padding 10px
				font-size 20px
				color #ffffff
	.content
		position relative
		padding 18px
		.title
			line-height 14px
			margin-bottom 8px
			font-size 16px
			font-weight 900
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
			font-weight 700
			line-height 24px
			.now
				margin-right 8px
				font-size 14px
				color rgb(240, 20, 20)
			.old
				text-decoration line-through
				font-size 10px
				color rgb(147, 153, 159)
		.cartcontrol-wrapper
			position absolute
			right 12px
			bottom 12px
		.buy
			position absolute
			right 18px
			bottom 18px
			z-index 10
			height 24px
			line-height 24px
			padding 0 12px
			box-sizing border-box
			border-radius 12px
			font-size 10px
			color #ffffff
			background-color rgb(0, 160, 220)
			opacity 1
			&.fade-enter-active, &.fade-leave-active
				transition all 0.2s
			&.fade-enter, &.fade-leave-active
				opacity 0
				z-index -1
	.info
		padding 18px
		.title
			line-height 14px
			margin-bottom 6px
			font-size 14px
			font-weight 400
			color rgb(7,17,27)
		.text
			line-height 24px
			padding 0 8px
			font-size 12px
			color rgb(77,85,93)
	.rating
		padding 18px
		.title
			line-height 14px
			margin-bottom 6px
			font-size 14px
			font-weight 400
			color rgb(7,17,27)

</style>
