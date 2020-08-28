<template>
	<view>
		
		<image src="../../static/images/commission-icon.png" mode="widthFix" class="p-fX top-0"></image>
		<view class="colorf p-r wh m-a mt-5">
			<image src="../../static/images/commission-icon1.png" mode="widthFix"></image>
			<view class="p-aXY text-center pt-4">
				<view class="font-80 bB-f d-inline-block">500</view>
				<view class="font-22 pt-1">押金</view>
			</view>
		</view>
		
		<view class="btn80 colorM"
		@click="Router.navigateTo({route:{path:'/pages/paySuccess/paySuccess'}})">立即付款</view>
		
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
.wh{width: 225rpx;height: 237rpx;}
.bB-f{border-bottom: 1px solid #fff;}
.btn80{margin: 250rpx 20rpx 0;background-color: #fff;border-radius: 10rpx;border: 1px solid #13C3F6;}
</style>
