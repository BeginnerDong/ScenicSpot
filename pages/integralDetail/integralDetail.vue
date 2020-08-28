<template>
	<view>
		
		<!-- banner -->
		<swiper class="swiper-box" indicator-dots="indicatorDots" autoplay="autoplay" interval="3000" indicator-active-color="#fff">
			<block>
				<swiper-item class="swiper-item" >
					<image src="../../static/images/integral-img1.png" mode="widthFix" />
				</swiper-item>
			</block>
		</swiper>
		
		<!-- 信息 -->
		<view class="px-2 py-3">
			<view class="font-32">服饰instax得力拍相机 一次成像相机 mini9 系列</view>
			<view class="flex1">
				<view class="font-20 colorR pt-2">
					<text class="font-30 font-w">266</text>积分
					<text class="font-30 font-w">+9.9</text>元
				</view>
				<view class="font-24 color6">库存 260</view>
			</view>
		</view>
		<view class="f5Bj-H20"></view>
		
		<!-- 详情 -->
		<view class="px-2 py-3">
			<view class="color6 pb-3">商品详情</view>
			<view class="text-center pb-4">
				商品名称......
			</view>
			<image src="../../static/images/integral-img2.png" mode="widthFix"></image>
		</view>
		
		
		<view style="height: 140rpx;"></view>
		<view class="flex1 p-2 bT-e1 p-fX bottom-0 bg-white">
			<view class="btn80 Mgb"
			@click="Router.navigateTo({route:{path:'/pages/onlineOrder/onlineOrder?type=1'}})">立即兑换</view>
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
.swiper-box,.swiper-item,swiper image{height: 750rpx;width: 100%;}
.btn80{color: #fff;border-radius: 10rpx;width: 100%;}
</style>
