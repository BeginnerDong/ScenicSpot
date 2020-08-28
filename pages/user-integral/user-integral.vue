<template>
	<view>
		
		<image src="../../static/images/commission-icon.png" mode="widthFix" class="p-fX top-0"></image>
		<view class="colorf p-r wh m-a top">
			<image src="../../static/images/commission-icon1.png" mode="widthFix"></image>
			<view class="p-aXY text-center pt-4">
				<view class="font-80 bB-f d-inline-block">500</view>
				<view class="font-22 pt-1">可用积分</view>
			</view>
		</view>
		
		<view class="line-h px-2 py-3 bB-f5 flex1" v-for="v in 4" :key="v">
			<view>
				<view>兑换商品</view>
				<view class="font-26 color8 pt-3">2020-08-12</view>
			</view>
			<view class="font-30 colorR">-55</view>
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
.top{margin-top: 60rpx;margin-bottom: 90rpx;}
.wh{width: 225rpx;height: 237rpx;}
.bB-f{border-bottom: 1px solid #fff;}
</style>
