<template>
	<view>
		
		<view class="list mb-3 shadowM">
			<view class="li" :class="liCurr==0?'on':''" @click="changeLi(0)">未使用</view>
			<view class="li" :class="liCurr==1?'on':''" @click="changeLi(1)">已使用</view>
			<view class="li" :class="liCurr==2?'on':''" @click="changeLi(2)">已过期</view>
		</view>
		
		<view class="mx-2 mb-3 p-r">
			<image src="../../static/images/coupons-icon.png" mode="widthFix" v-if="liCurr==0"></image>
			<image src="../../static/images/coupons-icon1.png" mode="widthFix" v-else></image>
			<view class="p-aXY flex">
				<view class="price1">5</view>
				<view class="txt line-h font-24 flex5 p-r pl-4 py-1" :class="liCurr==0?'b1':'b2'">
					<view class="font-30 font-w">包车专项</view>
					<view class="py-3">满120减5元</view>
					<view>使用时间：自领取90天内有效</view>
				</view>
			</view>
			<image src="../../static/images/coupons-icon2.png" class="icon" v-show="liCurr==1"></image>
			<image src="../../static/images/coupons-icon3.png" class="icon" v-show="liCurr==2"></image>
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
.price1{font-size: 100rpx;font-weight: bold;color: #FF3D12;width: 200rpx;text-align: center;}
.txt{height: 155rpx;}
.b1{border-left: 1px dashed #13C3F6;}
.b2{border-left: 1px dashed #1888;}
.icon{width: 97rpx;height: 94rpx;position: absolute;top: 0;right: 0;}
</style>
