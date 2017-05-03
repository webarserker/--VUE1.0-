<template>
	<div class="cartcontrol" v-show="shopcartkoop"  transition="cartcontrol"  >
		<div class="shoppingCart">
			<div class="shoppingCart-cart">购物车</div>
			<div class="shoppingCart-clear" @click="empty">清空</div>
		</div>
		
		<div class="cartcontrol-goods"  v-el:cart-control>
		  <ul>
			<li class="bale-parent" v-for="item in selectFoods" >
				<div class="bale-warpper">
					<div class="bale">{{item.name}}</div>
				    <div class="bale-text">¥{{item.price * item.count}}</div>
				    <addshow class="addshow" :food="foodelse[$index]"></addshow>
				</div>				    	
			</li>	
		  </ul>
		</div>
		  <div class="list-mask" v-show="shopcartkoop" transition="fade" @click="hide" ></div>
	</div>
	
</template>	

<script type="text/ecmascript-6">
	import BScroll from 'better-scroll';
	import addShow from 'components/addShow/addShow';
	import Vue from 'vue';
	
	export default {
		props: {
			selectFoods: {
				type: Array,
				default() {
					return [];
				}
			},
			selectIndex: {
				type: Number,
				default: 0
			},
			shopcartKoop: {
				type: Boolean,
				default: true
			}
		},
		computed: {
			name() {
				let name = '';
				this.selectFoods.forEach((food) => {
					name = food.name;
				});
				return name;
			},
			price() {
				let price = '';
				this.selectFoods.forEach((food) => {
					price = food.price;
				});
				console.log(price);
				return price;
			},
			count() {
				let count = '';
				return count;
			},
			foodelse() {
				var foodelse = this.selectFoods;
				if (!this.selectFoods) {
					Vue.set(this.food, 'count', 1);
				};
				return foodelse;
			},
			shopcartkoop() {
				this.$nextTick(() => {
				this._initScroll();
			});
				return this.shopcartKoop;
			}
		},
		methods: {
			_initScroll() {
				if (!this.scroll) {
					this.scroll = new BScroll(this.$els.cartControl, {
					click: true
				});
				} else {
					this.scroll.refresh();
				};
			},
			empty() {
				this.selectFoods.forEach((food) => {
					food.count = 0;
				});
				this.shopcartKoop = false;
			},
			drop(el) {
				console.log(el);
			},
			hide() {
				console.log(1);
				this.shopcartKoop = false;
			}
		},
		components: {
			addshow: addShow
		}
	};
</script>

<style lang="stylus" rel="stylesheet/stylus">
   .cartcontrol
     position: fixed
     font-family: "Arial","Microsoft YaHei","黑体","宋体",sans-serif
     bottom: 48px
     width: 100%
     z-index: 30
     .shoppingCart
       display: flex
       justify-content: space-between
       width: 100%
       height: 40px
       background: #f3f5f7
       line-height: 40px
       font-size: 14px
       border-bottom: 1px 
       .shoppingCart-cart
         padding-left: 35px
       .shoppingCart-clear
         padding-right: 35px
     .cartcontrol-goods
       background: #fff
       font-size: 14px
       padding: 0 35px 25px 35px
       max-height: 217px
       overflow: hidden
       .bale-parent
         position:relative
         display: flex
         flex-direction: row
         align-items: center   
         height: 49px
         .bale-warpper   
           display:flex
           width: 100%
           justify-content: space-between
           padding-right: 80px
	       .bale       
             font-size: 15px
             font-weight: 600
             color: rgb(7,17,27)
	       .addshow
	         bottom: 15px
	         right: 0px
	       .bale-text
	         font-size: 10px
	         color: rgb(240,20,20)
	         font-weight: 700  
 .list-mask
	position: fixed
	top: 0
	left: 0
	width: 100%
	height: 100%
	z-index: -1
	backdrop-filter: blur(10px)
	&.fade-transition
	  transition: all 0.5s
	  opacity: 1
	  background: rgba(7, 17, 27, 0.6)
	&.fade-enter, &.fade-leave
	  opacity: 0
	  background: rgba(7, 17, 27, 0)	         
</style>