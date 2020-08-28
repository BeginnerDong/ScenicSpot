<template>
	<view>
		
		<view class="pt-4 py-3 line-h"
		@click="Router.navigateTo({route:{path:'/pages/address/address'}})">
			<image src="../../static/images/thaorder-icon.png" mode="widthFix"></image>
			<view class="py-3 px-2">门店名称 15623587946</view>
			<view class="px-2">陕西省西安市长安区韦曲街道</view>
		</view>
		<view class="f5Bj-H20"></view>
		
		<view class="px-2 py-3 flex1">
			<image src="../../static/images/thaorder-img1.png" class="wh180 radius10"></image>
			<view class="h180 flex5 pl-2 flex-1 font-w">
				<view class="avoidOverflow2">西安秦始皇兵马俑博物馆+华清池+骊山+一日游【冰蛋语音讲解/长恨歌】</view>
				<view class="price1 font-34" v-show="type==0">2662</view>
				<view class="font-20 colorR pt-2" v-show="type==1">
					<text class="font-30 font-w">266</text>积分
					<text class="font-30 font-w">+9.9</text>元
				</view>
			</view>
		</view>
		<view class="f5Bj-H20"></view>
		
		<view v-show="type==0">
			<view class="px-2">
				<view class="py-4 bB-f5 flex1">
					<view>购买类型</view>
					<view>拼车</view>
				</view>
				<view class="py-4 bB-f5 flex1">
					<view>购买人数</view>
					<view class="flex color9 font-24">
						<view>请选择</view>
						<image src="../../static/images/icon.png" class="R-icon ml-1"></image>
					</view>
				</view>
				<view class="py-4 bB-f5 flex1">
					<view>套餐种类</view>
					<view>套餐A x2</view>
				</view>
				<view class="py-4 bB-f5 flex1">
					<view>套餐数量</view>
					<view class="count">
						<view class="countIcon"><image src="../../static/images/detailsonline-icon5.png" class="countIcon1"></image></view>
						<view class="countNum">1</view>
						<view class="countIcon"><image src="../../static/images/detailsonline-icon6.png" class="countIcon2"></image></view>
					</view>
				</view>
			</view>
			<view class="f5Bj-H20"></view>
		</view>
		
		
		<view style="height: 140rpx;"></view>
		<view class="flex1  p-fX bottom-0 bg-white shadowM">
			<view class="pl-3 flex">合计
				<text class="font-34 font-w price1 pl-1" v-show="type==0">3180</text>
				<view class="font-20 colorR pl-1" v-show="type==1">
					<text class="font-30 font-w">266</text>积分
					<text class="font-30 font-w">+9.9</text>元
				</view>
			</view>
			
			<view class="btn80 Mgb"
			@click="Router.navigateTo({route:{path:'/pages/deposit/deposit'}})">立即支付</view>
		</view>
		
		
		<!-- 人数弹窗 -->
		<view class="bg-mask" v-show="is_show">
			<view class="maskCon radius10 overflow-h ">
				<view class="p-3 flex1 bB-f5" v-for="v in 8" :key="v">
					<view>{{v}}人</view>
					<image src="../../static/images/thaorder-icon1.png" class="yes-icon"></image>
				</view>
			</view>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				is_show:false,
				type:0
			}
		},
		onLoad(options) {
			const self = this;
			self.type = options.type;
		},
		methods: {
			
		}
	}
</script>

<style scoped>
.btn80{color: #fff;border-radius: 0;width: 280rpx;line-height: 100rpx;}
.maskCon{width: 600rpx;margin-top: 25%;}
</style>
