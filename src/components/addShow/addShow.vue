<template>
	<div class="addCatch">		
		<div class="cart-decrease" v-show="food.count>0" @click="hideShow" transition="move">
			<span class="inner icon-remove_circle_outline"></span>
		</div>
		<div class="num" v-show="food.count>0">{{food.count}}</div>
		<div class="icon-add_circle" @click="addShow"></div>
	</div>
</template>	

<script type="text/ecmascript-6">
	import Vue from 'vue';
	
	export default {
		props: {
			food: {
				type: Object
			}
		},
		methods: {
			addShow(event) {
				if (!event._constructed) {
					return;
				}
				if (!this.food.count) {
					Vue.set(this.food, 'count', 1);
				} else {
					this.food.count++;
				}
				this.$dispatch('cart.add', event.target);
			},
			hideShow(event) {
				if (!event._constructed) {
					return;
				} else {
					this.food.count--;
				};
			}
		},
		created() {}
	};
</script>

<style lang="stylus" rel="stylesheet/stylus">
	.addCatch
	  position: absolute
	  display: flex
	  align-items: center
	  right: 18px
	  font-size: 24px
	  color: rgb(0,160,220)
	  bottom: 20px
	  .cart-decrease
	    transition: all 0.4s linear
	    &.move-transition
	      opacity: 1
	      transform: translate3d(0, 0, 0)
	      .inner
	        transition: all 0.4s linear
	        transform: rotate(0)
	    &.move-enter, &.move-leave
	      opacity: 0
	      transform: translate3d(24px, 0, 0)
	      .inner
	        transition: all 0.4s linear
	        transform: rotate(180deg)                     
	  .num
	    color: rgb(147,153,159)
	    font-size: 18px
	    margin:0 6px
	    transition: all 1s
	  .icon-add_circle
	    position: relative
</style>