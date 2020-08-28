<template>
	<view>
		
		<image src="../../static/images/my-img.png" mode="widthFix" class="p-fX top-0"></image>
		<view class="pl-3 pt-5">
			<image src="../../static/images/tehdriver-img.png" class="wh150 radius-5 mt-5 ml-2 shadow"></image>
			<view class="flex1 pl-2 line-h name">
				<view class="flex font-32">
					<view class="font-w">哆啦A梦</view>
					<view class="vipSign ml-1">会员</view>
				</view>
				<view class="flex font-24"
				@click="Router.navigateTo({route:{path:'/pages/realName/realName'}})">
					<view>实名认证</view>
					<image src="../../static/images/my-icon.png" class="wh100"></image>
				</view>
			</view>
		</view>
		
		<view class="mx-2 shadow mt-2">
			<view class="px-3 font-30 font-w py-4">常用工具</view>
			<view class="font-24 line-h color8 flex flex-wrap">
				<view class="pb-4 flex4 w-25"
				@click="Router.navigateTo({route:{path:'/pages/user-onlineOrder/user-onlineOrder'}})">
					<image src="../../static/images/my-icon4.png" class="wh50 mb-2"></image>
					<view>线路订单</view>
				</view>
				<view class="pb-4 flex4 w-25"
				@click="Router.navigateTo({route:{path:'/pages/user-integralOrder/user-integralOrder'}})">
					<image src="../../static/images/my-icon5.png" class="wh50 mb-2"></image>
					<view>积分订单</view>
				</view>
				<view class="pb-4 flex4 w-25"
				@click="Router.navigateTo({route:{path:'/pages/user-deposit/user-deposit'}})">
					<image src="../../static/images/my-icon6.png" class="yjIcon mb-2"></image>
					<view>我的押金</view>
				</view>
				<view class="pb-4 flex4 w-25"
				@click="Router.navigateTo({route:{path:'/pages/user-integral/user-integral'}})">
					<image src="../../static/images/my-icon2.png" class="wh50 mb-2"></image>
					<view>我的积分</view>
				</view>
				<view class="pb-4 flex4 w-25"
				@click="Router.navigateTo({route:{path:'/pages/user-coupon/user-coupon'}})">
					<image src="../../static/images/my-icon3.png" class="wh50 mb-2"></image>
					<view>我的优惠券</view>
				</view>
				<view class="pb-4 flex4 w-25"
				@click="Router.navigateTo({route:{path:'/pages/address/address'}})">
					<image src="../../static/images/my-icon1.png" class="dzIcon mb-2"></image>
					<view>收货地址</view>
				</view>
			</view>
		</view>
		
		<view class="mx-2 shadow mt-4">
			<view class="px-3 font-30 font-w py-4">第三方登录</view>
			<view class="font-24 line-h color8 flex flex-wrap">
				<view class="pb-4 flex4 w-25"
				@click="Router.navigateTo({route:{path:'/pages/driver/driver'}})">
					<image src="../../static/images/my-icon10.png" class="wh50 mb-2"></image>
					<view>司机入口</view>
				</view>
				<view class="pb-4 flex4 w-25"
				@click="Router.navigateTo({route:{path:'/pages/login/login'}})">
					<image src="../../static/images/my-icon8.png" class="wh50 mb-2"></image>
					<view>景区入口</view>
				</view>
				<view class="pb-4 flex4 w-25"
				@click="Router.navigateTo({route:{path:'/pages/entrance/entrance'}})">
					<image src="../../static/images/my-icon9.png" class="wh50 mb-2"></image>
					<view>商户入口</view>
				</view>
				<view class="pb-4 flex4 w-25">
					<image src="../../static/images/my-icon7.png" class="wh50 mb-2"></image>
					<view>代理入口</view>
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
			<view class="item" @click="Router.redirectTo({route:{path:'/pages/integralShop/integralShop'}})">
				<image src="../../static/images/nabar2.png" mode=""></image>
				<view>积分商城</view>
			</view>
			<view class="item on">
				<image src="../../static/images/nabar3-a.png" mode=""></image>
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

<style scoped>
.vipSign{width: 60rpx;line-height: 30rpx;border: 1px solid #13C3F6;color: #13C3F6;font-size: 22rpx;border-radius: 8rpx;text-align: center;}
.name{margin-top: -10rpx;}
.w-25{width: 25%;}
.dzIcon{width: 37rpx;height: 47rpx;}
.yjIcon{width: 52rpx;height: 48rpx;}
</style>
