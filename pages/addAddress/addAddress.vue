<template>
	<view class="px-2">
		
		<view class="py-4 bB-f5 flex1 line-h">
			<view>姓名</view>
			<input type="text" value="" placeholder="填写姓名" placeholder-style="color:#999;" />
		</view>
		<view class="py-4 bB-f5 flex1 line-h">
			<view>手机号</view>
			<input type="text" value="" placeholder="填写手机号" placeholder-style="color:#999;" />
		</view>
		<view class="py-4 bB-f5 flex1 line-h">
			<view>所在地区</view>
			<picker mode="region" class="flex-1 text-right">
				<view class="color9">请选择</view>
			</picker>
			<image src="../../static/images/icon.png" class="R-icon ml-1"></image>
		</view>
		<view class="py-4 bB-f5 flex1 line-h">
			<view>详细地址</view>
			<input type="text" value="" placeholder="请填写" placeholder-style="color:#999;" />
		</view>
		<view class="py-4 bB-f5 flex1 line-h">
			<view>设为默认地址</view>
			<image src="../../static/images/address-icon5.png" class="mr-icon"></image>
			<!-- <image src="../../static/images/address-icon6.png" class="mr-icon"></image> -->
		</view>
		
		<view class="btn80 Mgb colorf mt-4">确定</view>
		
		
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				type:0
			}
		},
		onLoad(options){
			const self = this;
			self.type = options.type
		},
		methods: {
			
		}
	}
</script>

<style scoped>
input{flex: 1;text-align: right;line-height: 1;font-size: 22rpx;height: 22rpx;}
.mr-icon{width: 67rpx;height: 34rpx;}
</style>
