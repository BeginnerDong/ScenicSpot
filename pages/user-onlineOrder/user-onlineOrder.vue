<template>
	<view>
		
		<view class="list mb-2 shadowM">
			<view class="li" :class="liCurr==0?'on':''" @click="changeLi(0)">全部</view>
			<view class="li" :class="liCurr==1?'on':''" @click="changeLi(1)">待完成</view>
			<view class="li" :class="liCurr==2?'on':''" @click="changeLi(2)">已完成</view>
		</view>
		
		<view class="bg-white radius10 mx-2 px-2"
		@click="Router.navigateTo({route:{path:'/pages/onlineOrder-detail/onlineOrder-detail'}})">
			<view class="font-24 color8 flex1 py-2">
				<view>交易时间：2019.12.30</view>
				<view class="colorR">已完成</view>
			</view>
			<view class="flex1 mb-2">
				<image src="../../static/images/thaorder-img1.png" class="wh180 radius10"></image>
				<view class="h180 flex5 pl-2 flex-1">
					<view class="avoidOverflow2 font-w">西安秦始皇兵马俑博物馆+华清池+骊山+一日游【冰蛋语音讲解/长恨歌】</view>
					<view class="flex1">
						<view class="price1 font-34 font-w">2662</view>
						<view>4人</view>
					</view>
				</view>
			</view>
			<view class="flex1 py-3">
				<view>套餐：</view>
				<view>包车套餐（A套餐） x2</view>
			</view>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
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
.li{width: 33.33%;}
</style>
