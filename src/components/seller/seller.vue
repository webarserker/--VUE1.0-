<template>
	
	<div class="seller" v-el:seller>
		<div class="seller-content" >
			<div class="overview">
			<h1 class="title">{{seller.name}}</h1>
			<div class="desc border-1px">
				<star :size="36" :score="seller.score"></star>
				<span class="text">({{seller.ratingCount}})</span>
				<span class="text">月销售{{seller.sellCount}}单</span>
			</div>
			
	   <ul class="remark">
          <li class="block">
            <h2>起送价</h2>
            <div class="content">
              <span class="stress">{{seller.minPrice}}</span>元
            </div>
          </li>
          <li class="block">
            <h2>商家配送</h2>
            <div class="content">
              <span class="stress">{{seller.deliveryPrice}}</span>元
            </div>
          </li>
          <li class="block">
            <h2>平均配送时间</h2>
            <div class="content">
              <span class="stress">{{seller.deliveryTime}}</span>分钟
            </div>
          </li>
       </ul>       
	   <div class="favorite" @click="toggleFavorite">
	   	<span class="icon-favorite" :class="{'active':favorite}"></span>
	   	<span class="text">{{favoriteText}}</span>
	   </div>
			</div>
		<split></split>
		<div class="bulletin">
        <h1 class="title">公告与活动</h1>
        <div class="content-wrapper border-1px">
          <p class="content">{{seller.bulletin}}</p>
        </div>
			
			<ul v-if="seller.supports" class="supports border-1px">
            	<li class="supports-item" v-for="item in seller.supports">
            		<span class="icon" :class="classMap[seller.supports[$index].type]"></span>
            		<span class="text">{{seller.supports[$index].description}}</span>                    	
            	</li>
            </ul>
		<split></split>
		<div class="pics">
			<h1 class="title">商家实景</h1>
			<div class="pics-wrapper" v-el:pic-wrapper>
				<ul class="pic-list" v-el:pic-list>
					<li class="pic-item" v-for="pic in seller.pics">
						<img :src="pic" width="120" height="90" />
					</li>
				</ul>
			</div>
		</div>
		<split></split>
		<div class="info">
			<div class="title">商家信息</div>
			<ul>
				<li class="info-item" v-for="info in seller.infos">{{info}}</li>
			</ul>
		</div>
		</div>
		<shopcart :select-foods="selectFoods" :delivery-price="seller.deliveryPrice" :min-price="seller.minPrice"></shopcart>
	</div>
</template>

<script type="text/ecmascript-6">
	import star from 'components/star/star';
	import {saveTolocal, loadFromLocal} from 'common/js/store';
	import split from 'components/split/split';
	import BScroll from 'better-scroll';
	import shopcart from 'components/shopcart/shopcart';
	
	export default {
		props: {
			seller: {
				type: Object
			}
		},
		data() {
			return {
				favorite: loadFromLocal(this.seller.id, 'favorite', false)
			};
		},
		computed: {
			favoriteText() {
				return this.favorite ? '已搜藏' : '收藏';
			}
			},
		created() {
			this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
		},
		watch: {
			'seller'() {
              this._initScroll();
              this._initPics();
       }
		},
		ready() {
			this._initScroll();
			this._initPics();
		},
		methods: {
			toggleFavorite(event) {
				if (!event._constructed) {
					return;
				};
				this.favorite = !this.favorite;
				saveTolocal(this.seller.id, 'favorite', this.favorite);
			},
			_initScroll() {
				if (!this.scroll) {
					this.scroll = new BScroll(this.$els.seller, {
							click: true
				});
				} else {
					this.scroll.refresh();
				}
			},
			_initPics() {
				if (this.seller.pics) {
				let picWidth = 120;
				let margin = 6;
				let width = (picWidth + margin) * this.seller.pics.length - margin;
				this.$els.picList.style.width = width + 'px';
				this.$nextTick(() => {
					if (!this.picscroll) {
						this.picscroll = new BScroll(this.$els.picWrapper, {
							scrollX: true,
							click: true,
							eventPassthrough: 'vertical'
				});
					} else {
						this.picscroll.refresh();
					};
				});
			};
			}
		},
		components: {
			star: star,
			split: split,
			shopcart: shopcart
		}
	};
</script>
 
<style lang="stylus" rel="stylesheet/stylus">
 @import "../../common/stylus/mixin.styl"

	.seller
	  position: absolute
	  top: 174px
	  bottom:0
	  left:0
	  width:100%
	  overflow:hidden
	  .overview
	    padding: 18px 0 0 18px;
	    .title
	      font-size: 14px
	      color: rgb(7,17,27)
	      line-height: 14px
	      margin-bottom: 8px
	      font-weight: bold;
	    .desc
	      display: flex
	      flex-direction: row
	      padding-bottom: 18px
	      font-size: 0
	      border-1px(rgba(7,17,27,.1))
	      .star
	        display: inline-block      
	        vertical-align: top
	        margin-right: 8px 
	      .text
	        display: inline-block   
	        vertical-align: top
	        margin-right: 12px
	        font-size: 10px
	        color: rgb(77,85,93)
	        line-height: 18px
	    .remark
	      display: flex
	      flex-direction: row
	      font-size: 10px
	      padding: 18px 0 18px 0
	      .block
	        flex: 1
	        text-align: center
	        border-right: 1px solid rgba(7,17,27,.1)
	        &:last-child
	          border: none    
	      h2
	        font-size: 10px
	        color: rgb(147,153,159)
	        line-height: 10px
	        margin-bottom: 4px
	      .content span
	        font-size: 24px
	        font-weight: 200
	        color:rgb(7,17,27)
	        line-height: 24px  
	    .favorite
	      position: absolute
	      width: 50px
	      right: 11px
	      top: 18px
	      text-align: center
	      .icon-favorite
	        display: block
	        margin-bottom: 4px
	        line-height: 24px
	        font-size: 24px
	        color: #d4d6d9 
	        &.active
	          color: rgb(240,20,20)
	      .text
	        line-height: 10px
	        font-size: 10px 
	        color: rgb(77,85,93)   
	   .bulletin       
	     padding: 18px 0 16px 18px
	     border-1px(rgba(7,17,27,.1))
	     .title
	       font-size: 14px
	       color: rgb(7,17,27)
	       line-height: 14px
	       margin-bottom: 8px
	       font-weight: bold
	     .content-wrapper
	       padding: 0 12px 16px 0 
	       border-1px(rgba(7,17,27,.1))
	       .content
	         font-size: 12px
	         font-weight: 200
	         color: rgb(240,20,20)
	         line-height: 24px   
	     .supports
	       width:80%
	       .supports-item
             padding: 16px 12px 16px 0 
             margin-bottom:12px
             font-size:0
             border-1px(rgba(7,17,27,.1))
             &.last-child
               border: none
             .text
               line-height:16px
               font-size:12px
               font-weight: 200
               color: rgb(7,17,27)   
	         .icon
	           display: inline-block
	           vertical-align:top  
	           width:12px
	           height:12px
	           margin-right:4px
	           background-size:12px 12px
	           background-repeat:no-repeat
	           &.decrease
	             bg-image('decrease_4')
	           &.discount
                 bg-image('discount_4')
               &.guarantee
                 bg-image('guarantee_4')  
               &.invoice
                 bg-image('invoice_4')
               &.special
                 bg-image('special_4')  
       .pics
         padding: 18px 0 12px 0
         .title
           font-size: 14px
           color: rgb(7,17,27)
           line-height: 14px
           margin-bottom: 8px
           font-weight: bold
         .pics-wrapper
           width: 100%
           overflow: hidden
           white-space: nowrap
           .pic-list
             font-size: 0
             .pic-item
               display: inline-block
               margin-right: 6px
               width: 120px
               height: 90px
               &:last-child
                 marign: 0
       .info
         padding: 18px 0 12px 0
         font-size: 0px   
         .title
           font-size: 14px
           color: rgb(7,17,27)
           line-height: 14px
           padding-bottom: 12px
           font-weight: bold     
           border-1px(rgba(7,17,27,.1))  
         .info-item
           border-1px(rgba(7,17,27,.1))
           padding: 16px 0px 16px 12px   
           font-size: 12px
           font-weight: 200
           color: rgb(7,17,27)
           line-height: 16px
           &.last-child
               border-none()
</style>  