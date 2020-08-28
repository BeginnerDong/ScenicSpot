<template>
	<view>
		
		<view class="bg-white m-2 mb-0 px-2 radius10 line-h">
			<view class="pt-3 pb-4 font-34 font-w flex1">
				<view class="tit p-r t-indent20">套餐A</view>
				<!-- <image src="../../static/images/detailsonline-icon3.png" class="wh46"></image> -->
				<image src="../../static/images/detailsonline-icon4.png" class="wh46"></image>
			</view>
			<view class="pb-4 flex1">
				<view>路线A+路线B+路线C</view>
				<view>￥125</view>
			</view>
			<view class="text-right py-4 bT-f5">
				总计：<text class="price">56</text>
			</view>
		</view>
		
		
		
		<view style="height: 140rpx;"></view>
		<view class="flex1 p-2 bT-f5 p-fX bottom-0 bg-white">
			<view class="btn80 Mgb"
			@click="Router.navigateTo({route:{path:'/pages/onlineOrder/onlineOrder?type=0'}})">确定</view>
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

<style>
page{background: #f5f5f5;}
</style>
<style scoped>
.tit::before{content: '';width: 8rpx;height: 20rpx;background-color: #13C3F6;position: absolute;top: 0;left: 0;}
.btn80{color: #fff;border-radius: 10rpx;width: 100%;}
</style>