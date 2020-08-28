<template>
	<view>
		
		<view v-for="v in 3" :key="v">
			<view class="line-h px-2">
				<view class="bB-f5 py-3">
					<view class="pb-2">张丹丹 <text class="color6 pl-3">15623587946</text></view>
					<view class="color6">陕西省西安市长安区韦曲街道</view>
				</view>
				<view class="font-24 color9 flex1 py-3">
					<view class="flex">
						<image src="../../static/images/address-icon3.png" class="wh32 mr-1"></image>
						<!-- <image src="../../static/images/address-icon2.png" class="wh32 mr-1"></image> -->
						<view>默认地址</view>
					</view>
					<view class="flex">
						<view class="flex pr-5"
						@click="Router.navigateTo({route:{path:'/pages/addAddress/addAddress?type=1'}})">
							<image src="../../static/images/address-icon.png" class="wh26 mr-1"></image>
							<view>编辑</view>
						</view>
						<view class="flex">
							<image src="../../static/images/address-icon1.png" class="wh26 mr-1"></image>
							<view>删除</view>
						</view>
					</view>
				</view>
			</view>
			<view class="f5Bj-H20"></view>
		</view>
		
		
		
		
		<view style="height: 130rpx;"></view>
		<view class="flex0 py-3 bT-e1 p-fX bottom-0"
		@click="Router.navigateTo({route:{path:'/pages/addAddress/addAddress?type=0'}})">
			<image src="../../static/images/address-icon4.png" class="wh26 mr-1"></image>
			<view>新增地址</view>
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
.signM{width: 50rpx;line-height: 30rpx;font-size: 20rpx;color: #E6B968;border: 1px solid #E6B968;border-radius: 5rpx;text-align: center;}
.w475{width: 475rpx;}
</style>
