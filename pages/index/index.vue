<template>
	<view>
		<!-- 定位 -->
		<view class="flex1 font-24 p-2">
			<view class="flex">
				<image src="../../static/images/home-icon.png" class="dw-icon mr-1"></image>
				<view>雁塔区</view>
			</view>
			<view class="flex"
			@click="Router.navigateTo({route:{path:'/pages/store/store'}})">
				<view>军干所社区</view>
				<image src="../../static/images/home-icon1.png" class="sj-icon ml-1"></image>
			</view>
		</view>
		<!-- banner -->
		<swiper class="swiper-box mx-2 py-1" indicator-dots="indicatorDots" autoplay="autoplay" interval="3000" indicator-active-color="#fff">
			<block>
				<swiper-item class="swiper-item" >
					<image src="../../static/images/home-banenr.png" mode="widthFix" />
					<!-- <video src="" controls></video> -->
				</swiper-item>
			</block>
		</swiper>
		
		<!-- 优惠券 -->
		<view class="p-3 p-r colorf">
			<image src="../../static/images/home-img.png" mode="widthFix"></image>
			<view class="p-aXY mx-3 d-flex j-end a-center">
				<view class="font-20"><text class="font-80 font-w">50</text>元</view>
				<viwe class="text-right line-h">
					<view class="font-24 pb-2">满100可用</view>
					<view class="font-20">(包车专享)</view>
				</viwe>
				<view class="btn50 shadowM mx-4"  @click="isShow">立即领取</view>
			</view>
		</view>
		
		<!-- 线路推荐 -->
		<view class="font-30 font-w px-2 pt-2">线路推荐</view>
		<view class="mx-2 pt-4 flex1" v-for="v in 3" :key="v"
		@click="Router.navigateTo({route:{path:'/pages/onlineDetail/onlineDetail'}})">
			<image src="../../static/images/home-img1.png" class="wh240 radius10"></image>
			<view class="h240 flex5 pl-2 flex-1">
				<view class="font-30 avoidOverflow2">西安秦始皇兵马俑博物馆+华清池+ 骊山+一日游【冰蛋语音讲解/长120小时】</view>
				<view class="font-24 color8 flex-1 pt-2">在西安让您感受千年古都的文化气息</view>
				<view>
					<text class="price1 font-34 font-w">2662</text>
					<text class="font-20 color9 pl-2">已售 235</text>
				</view>
			</view>
		</view>
		
		<!-- 优惠券领取 -->
		<view class="bg-mask" v-show="is_show">
			<view class="hb p-r">
				<image src="../../static/images/home-icon3.png" mode="widthFix"></image>
				<view class="colorf font-50 text-center p-aXY mm">
					<view>满100减50</view>
					<view class="font-34 pt-4">领取成功</view>
				</view>
			</view>
			<image src="../../static/images/home-icon4.png" class="wh60 m-a" @click="isShow"></image>
		</view>
		
		
		
		<view style="height: 130rpx;"></view>
		<!-- footer -->
		<view class="footer">
			<view class="item on">
				<image src="../../static/images/nabar1-a.png" mode=""></image>
				<view>首页</view>
			</view>
			<view class="item" @click="Router.redirectTo({route:{path:'/pages/integralShop/integralShop'}})">
				<image src="../../static/images/nabar2.png" mode=""></image>
				<view>积分商城</view>
			</view>
			<view class="item" @click="Router.redirectTo({route:{path:'/pages/user/user'}})">
				<image src="../../static/images/nabar3.png" mode=""></image>
				<view>我的</view>
			</view>
		</view>
		
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				is_show: false,
			}
		},
		onLoad() {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			getMainData() {
				const self = this;
				console.log('852369')
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				self.$apis.orderGet(postData, callback);
			},
			
			isShow(){
				const self = this;
				self.is_show = !self.is_show;
			}
			
		}
	};
</script>

<style scoped>
.swiper-box,.swiper-item,swiper image,video{width: 710rpx;height: 300rpx;border-radius: 10rpx;}

.btn50{width: 150rpx;background-image: linear-gradient(to right,#FF5F05,#FEC011);}
.hb{width: 440rpx;height: 554rpx;margin: 30% auto 20%;}
.mm{margin-top: 300rpx;}
</style>