<template>
	<view>
		
		<view class="list mb-2 shadowM">
			<view class="li" :class="liCurr==0?'on':''" @click="changeLi(0)">全部</view>
			<view class="li" :class="liCurr==1?'on':''" @click="changeLi(1)">待发货</view>
			<view class="li" :class="liCurr==2?'on':''" @click="changeLi(2)">待收货</view>
			<view class="li" :class="liCurr==3?'on':''" @click="changeLi(3)">已收货</view>
		</view>
		
		<view class="bg-white radius10 mx-2 px-2">
			<view class="font-24 color8 flex1 py-2">
				<view>交易时间：2019.12.30</view>
				<view class="colorR">待收货</view>
			</view>
			<view class="flex1"
			@click="Router.navigateTo({route:{path:'/pages/orderDetail/orderDetail'}})">
				<image src="../../static/images/thaorder-img1.png" class="wh180 radius10"></image>
				<view class="h180 flex5 pl-2 flex-1">
					<view class="avoidOverflow2 font-w">西安秦始皇兵马俑博物馆+华清池+骊山+一日游【冰蛋语音讲解/长恨歌】</view>
					<view class="flex1">
						<view class="font-20 colorR pt-2">
							<text class="font-30 font-w">266</text>积分
							<text class="font-30 font-w">+9.9</text>元
						</view>
						<view>x1</view>
					</view>
				</view>
			</view>
			<view class="py-2 text-right">
				<view class="btn text-center font-26 color8 b-e1 radius10">确认收货</view>
			</view>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				liCurr:0
			}
		},
		methods: {
			
			changeLi(i){
				const self = this;
				self.liCurr = i;
			}
			
		}
	}
</script>
<style>page{background-color: #f5f5f5;}</style>
<style scoped>
.li{width: 25%;}
.btn{width: 150rpx;line-height: 60rpx;display: inline-block;}
</style>
