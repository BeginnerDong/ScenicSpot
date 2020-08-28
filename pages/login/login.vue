<template>
	<view>
		
		<image src="../../static/images/landing-img.png" mode="widthFix" class="p-s top-0 "></image>
		
		<view class="d-flex a-start px-5 mx-1 mt-5 pt-3">
			<image src="../../static/images/zlanding-icon1.png" class="phone"></image>
			<input type="text" value="" placeholder="输入账号" placeholder-style="color:#999" />
		</view>
		<view class="d-flex a-start px-5 mx-1 mt-5 pt-3">
			<image src="../../static/images/landing-icon.png" class="safe"></image>
			<input type="text" value="" placeholder="输入密码" placeholder-style="color:#999" />
		</view>
		
		<view class="btn80 Mgb">登录</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				
			}
		},
		methods: {
			
		}
	}
</script>

<style scoped>
input{font-size: 24rpx;margin-left: 30rpx;flex: 1;border-bottom: 1px solid #f5f5f5;padding: 15rpx 0;}
.phone{width: 29rpx;height: 43rpx;}
.safe{width: 36rpx;height: 40rpx;}
.btn80{color: #fff;border-radius: 10rpx;width: 100%;margin: 100rpx auto;width: 640rpx;}
</style>
