<template>
	<view>
		
		<!-- banner -->
		<swiper class="swiper-box" indicator-dots="indicatorDots" autoplay="autoplay" interval="3000" indicator-active-color="#fff">
			<block>
				<swiper-item class="swiper-item" >
					<image src="../../static/images/detailsonline-img.png" mode="widthFix" />
				</swiper-item>
			</block>
		</swiper>
		
		<!-- 详情信息 -->
		<view class="px-2 py-3">
			<view class="font-32">西安秦始皇兵马俑博物馆+华清池+骊山+一日游【冰蛋语音讲解/长恨歌】</view>
			<view class="font-24 color8 flex-1 pt-2 pb-3">在西安让您感受千年古都的文化气息</view>
			<view class="flex1">
				<view>
					<text class="price1 font-34 font-w">2662</text>
					<text class="font-20 color9 pl-2">已售 235</text>
				</view>
				<view class="font-24 colorM">可得积分 26</view>
			</view>
		</view>
		<view class="f5Bj-H20"></view>
		
		<!-- 拼团 -->
		<view class="px-2 py-3">
			<view class="font-22 color8">这些人刚刚购买成功，可参与拼团</view>
			<view class="flex1 pt-3">
				<image src="../../static/images/detailsonline-img1.png" class="wh80"></image>
				<view class="font-24 pl-2 flex-1">月亮的儿童</view>
				<view class="font-26">3人</view>
			</view>
		</view>
		<view class="f5Bj-H20"></view>
		
		<!-- 旅游行程安排 -->
		<view class="px-2 py-3">
			<view class="font-34 font-w">旅游行程安排</view>
			<view class="flex py-2 mb-2">
				<view class="flex pr-5">
					<image src="../../static/images/detailsonline-icon1.png" class="xcIcon"></image>
					<view class="font-24 pl-2">行程安排 <text class="font-32 font-w pl-1"> 2</text></view>
				</view>
				<view class="flex pl-5">
					<image src="../../static/images/detailsonline-icon.png" class="xcIcon"></image>
					<view class="font-24 pl-2">观光点数 <text class="font-32 font-w pl-1"> 8</text></view>
				</view>
			</view>
			<view class="online d-flex a-start j-sb line-h pl-3 p-r pb-2 mt-1" v-for="v in 2" :key="v">
				<view class="time colorM font-32 font-w flex-1">10点</view>
				<view class="flex flex-wrap onlineCon">
					<view class="flex pb-2" v-for="v in 5" :key="v">
						<view>静安寺</view>
						<image src="../../static/images/detailsonline-icon2.png" class="zx-icon mx-2"></image>
					</view>
					<view class="flex pb-2">
						<view>上海博物馆</view>
					</view> 
				</view>
			</view>
		</view>
		<view class="f5Bj-H20"></view>
		
		<!-- 套餐 -->
		<view class="font-24 px-2">
			<view>
				<view class="flex1 py-4">
					<view class="font-34 font-w">包车套餐</view>
					<view class="flex color9">
						<view>查看详情</view>
						<image src="../../static/images/icon.png" class="R-icon ml-1"></image>
					</view>
				</view>
				<view class="pt-1 bB-e1 flex flex-wrap tcBox">
					<view class="tc flex0 p-r b-e1 radius10 mb-4 on">
						<view class="font-44 font-w pt-2 price1">88</view>
						<view class="tcSign">套餐A</view>
					</view>
				</view>
			</view>
			<view>
				<view class="flex1 py-4">
					<view class="font-34 font-w">拼车套餐</view>
					<view class="flex color9">
						<view>查看详情</view>
						<image src="../../static/images/icon.png" class="R-icon ml-1"></image>
					</view>
				</view>
				<view class="pt-1 flex flex-wrap tcBox">
					<view class="tc flex0 p-r b-e1 radius10 mb-4">
						<view class="font-44 font-w pt-2 price1">88</view>
						<view class="tcSign">套餐A</view>
					</view>
				</view>
			</view>
		</view>
		<view class="f5Bj-H20"></view>
		
		<!-- 服务协议 -->
		<view class="font-24 fw">
			点击购买表示已经同意 
			<text class="colorR"
			@click="Router.navigateTo({route:{path:'/pages/service/service'}})">《服务协议》</text>
		</view>
		
		
		<!-- 按钮 -->
		<view class="flex1 p-2 bT-f5 p-fX bottom-0 bg-white">
			<view class="btn80 Mgb"
			@click="Router.navigateTo({route:{path:'/pages/setMealDetail/setMealDetail'}})">购买拼车</view>
			<view class="btn80 Mgb"
			@click="Router.navigateTo({route:{path:'/pages/setMealDetail/setMealDetail'}})">购买包车</view>
		</view>
		
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router
			}
		},
		methods: {
			
		}
	}
</script>

<style scoped>
.swiper-box,.swiper-item,swiper image{height: 500rpx;width: 100%;}
.xcIcon{width: 41rpx;height: 36rpx;}
.ggIcon{width: 38rpx;height: 36rpx;}

.online image{display: inline-block;}
.onlineCon{width: 600rpx;}
.time::before{content: '';border-radius: 50%;border: 1px solid #888;width: 12rpx;height: 12rpx;position: absolute;top: 6rpx;left: 0;}
.online::before{content: '';border-left: 1px solid #888;width: 1rpx;height: 65%;position: absolute;left: 6rpx;bottom: 10rpx;}

.tc{width: 220rpx;height: 128rpx;margin-right: 25rpx;background-color: #f5f5f5;}
.tc:nth-child(3){margin-right: 0;}
.tcSign{position: absolute;width: 90rpx;line-height: 40rpx;text-align: center;border-radius: 10rpx 0 10rpx 0;border: 1px solid #f1f1f1;background-color: #fff;top: -20rpx;left: 50%;margin-left: -45rpx;}
.on{background-color: #13C3F6;border: none;}
.on .tcSign{border: 1px solid #13C3F6;color: #13C3F6;}
.on .price1{color: #fff;}

.fw{padding: 80rpx 0 140rpx;text-align: center;}
.btn80{color: #fff;border-radius: 10rpx;width: 340rpx;}
</style>
