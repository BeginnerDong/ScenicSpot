<template>
	<view class="px-2">
		
		<view class="px-2 py-3 flex1 bg-white radius10 mt-2">
			<image src="../../static/images/thaorder-img1.png" class="wh180 radius10"></image>
			<view class="h180 flex5 pl-2 flex-1 font-w">
				<view class="avoidOverflow2">西安秦始皇兵马俑博物馆+华清池+骊山+一日游【冰蛋语音讲解/长恨歌】</view>
				<view class="flex1">
					<view class="price1 font-34 font-w">2662</view>
					<view>4人</view>
				</view>
			</view>
		</view>
		
		<view class="bg-white mt-3  px-2 radius10 line-h">
			<view class="pt-3 pb-4 flex1 bB-de1">
				<view class="font-w">包车套餐A（A套餐）</view>
				<view class="color9 font-26">x2</view>
			</view>
			<view class="bB-de1 pt-4">
				<view class="pb-4 flex1" v-for="v in 4" :key="v">
					<view>路线A+路线B+路线C</view>
					<view class="color9 font-26">x2</view>
				</view>
			</view>
			<view class="py-3 flex1">
				<view>总计：</view>
				<text class="price">88</text>
			</view>
		</view>
	
		
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

<style>
page{background-color: #f5f5f5;}
</style>
