<template>
	<view>
		
		<view class="px-2 pb-2 flex1">
			<view class="w345 radius10 overflow-h p-r bg-white mt-2"
			@click="Router.navigateTo({route:{path:'/pages/integralDetail/integralDetail'}})">
				<image src="../../static/images/integral-img.png" class="img"></image>
				<view class="kcSign">库存 230</view>
				<view class="p-2">
					<view class="avoidOverflow2">服饰instax得力拍相机 一次成像相机 mini9 系列</view>
					<view class="font-20 colorR pt-2">
						<text class="font-30 font-w">266</text>积分
						<text class="font-30 font-w">+9.9</text>元
					</view>
				</view>
			</view>
		</view>
		
		
		
		<view style="height: 130rpx;"></view>
		<!-- footer -->
		<view class="footer">
			<view class="item" @click="Router.redirectTo({route:{path:'/pages/index/index'}})">
				<image src="../../static/images/nabar1.png" mode=""></image>
				<view>首页</view>
			</view>
			<view class="item on">
				<image src="../../static/images/nabar2-a.png" mode=""></image>
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
				Router:this.$Router
			}
		},
		methods: {
			
		}
	}
</script>
<style>page{background-color: #f5f5f5;}</style>
<style scoped>
.w345{width: 345rpx;}
.img{width: 345rpx;height: 345rpx;}
.kcSign{line-height: 40rpx;padding: 0 10rpx;background-color: rgba(0,0,0,0.5);color: #fff;font-size: 24rpx;border-radius: 10rpx 0 10rpx 0;position: absolute;top: 0;left: 0;}
</style>
