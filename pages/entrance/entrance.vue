<template>
	<view class="pb-5">
		
		<view class="p-r p-2">
			<view class="Mgb p-aX top-0" style="height: 240rpx;"></view>
			<view class="p-r font-30 colorf line-h flex">
				<image src="../../static/images/tehdriver-img.png" class="wh110 my-2"></image>
				<view class="pl-2">
					<view class="pb-3 font-w">六六六</view>
					<view class="sign">代理</view>
					<!-- <view class="sign">景点</view> -->
					<!-- <view class="sign">商户</view> -->
				</view>
				<view class="font-26 p-a top-0 right-0">退出</view>
			</view>
		</view>
		
		<view class="bg-white radius10 font-24 line-h p-r mx-2 shadow">
			<view class="flex4 py-3 p-r">
				<view class="font-40 font-w pb-2">12</view>
				<view class="color6">分润额(元)</view>
			</view>
		</view>
		
		<view class="flex line-h bg-white bB-f5 mt-1">
			<view class="flex0 w-50 py-3">
				<view>开始时间</view>
				<image src="../../static/images/tehdriver-icon.png" class="xjt-ixon ml-1"></image>
			</view>
			<view class="flex0 w-50 py-3">
				<view>结束时间</view>
				<image src="../../static/images/tehdriver-icon.png" class="xjt-ixon ml-1"></image>
			</view>
		</view>
		
		<view class="px-2 line-h bg-white">
			<view class="flex1 py-4 bB-f5">
				<view class="avoidOverflow pr-5 flex-1">商品名称</view>
				<view class="colorR pl-5">+55</view>
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

<style>
page{background-color: #fff;}
</style>
