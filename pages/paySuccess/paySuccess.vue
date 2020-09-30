<template>
	<view>
		
		<image src="../../static/images/successful-img.png" mode="widthFix" class="p-fX top-0"></image>
		<view class="font-40 colorf text-center p-r cp">车牌号：{{mainData.car_no?mainData.car_no:''}}</view>
		<image src="../../static/images/successful-icon.png" class="yes"></image>
		<view class="font-26 text-center">支付成功</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				mainData:{}
			}
		},
		onLoad(options) {
			const self = this;
			self.id = options.id;
			self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			
			getMainData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					id:self.id
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData = res.info.data[0];
					}
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.orderGet(postData, callback);
			},
			
		}
	}
</script>

<style scoped>
.cp{margin: 110rpx 0;}
.yes{width: 231rpx;height: 220rpx;margin: 250rpx auto 40rpx;}
</style>
