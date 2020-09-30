<template>
	<view>
		
		<image src="../../static/images/commission-icon.png" mode="widthFix" class="p-fX top-0"></image>
		<view class="colorf p-r wh m-a mt-5">
			<image src="../../static/images/commission-icon1.png" mode="widthFix"></image>
			<view class="p-aXY text-center pt-4">
				<view class="font-80 bB-f d-inline-block">{{money}}</view>
				<view class="font-22 pt-1">押金</view>
			</view>
		</view>
		
		<view class="btn80 colorM"
		@click="addOrder">立即付款</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				money:''
			}
		},
		
		onLoad(){
			const self = this;
			uni.showLoading();
			const callback = (res) => {
				self.money = uni.getStorageSync('user_info').thirdApp.deposit;
				uni.hideLoading();
			};
			self.$Token.getProjectToken(callback, {
				refreshToken: true
			})
		},
		
		
		methods: {
			
			addOrder() {
				const self = this;
				uni.setStorageSync('canClick', false);
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.data = {
					price: parseFloat(self.money).toFixed(2),
					type: 6,
					level:1
				};
				const callback = (res) => {
					if (res.solely_code == 100000) {
						self.orderId = res.info.id;
						self.pay()
					} else {
						self.$Utils.showToast(res.msg, 'none')
					}
				};
				self.$apis.addVirtualOrder(postData, callback);
			},
			
			
			pay() {
				const self = this;
				uni.setStorageSync('canClick', false);
				const postData = {};
				postData.wxPay = {
					price: parseFloat(self.money).toFixed(2),
				};
				postData.tokenFuncName = 'getProjectToken',
				postData.searchItem = {
					id: self.orderId
				};
				const callback = (res) => {
					if (res.solely_code == 100000) {
						if (res.info) {
							const payCallback = (payData) => {
								console.log('payData', payData)
								if (payData == 1) {
									self.$Utils.showToast('支付成功', 'none');
									setTimeout(function() {
										uni.navigateBack({
											delta: 1
										});
									}, 1000);
								} else {
									uni.setStorageSync('canClick', true);
									self.$Utils.showToast('支付失败', 'none');
								};
							};
							self.$Utils.realPay(res.info, payCallback);
						} else {
							self.$Utils.showToast('支付成功', 'none');
							setTimeout(function() {
								uni.navigateBack({
									delta: 1
								});
							}, 1000);
						};
					} else {
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast(res.msg, 'none');
					};
				};
				self.$apis.pay(postData, callback);
			},
			
		}
	}
</script>

<style scoped>
.wh{width: 225rpx;height: 237rpx;}
.bB-f{border-bottom: 1px solid #fff;}
.btn80{margin: 250rpx 20rpx 0;background-color: #fff;border-radius: 10rpx;border: 1px solid #13C3F6;}
</style>
