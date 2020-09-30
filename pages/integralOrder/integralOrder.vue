<template>
	<view>
		
		<view class="pt-4 py-3 line-h" v-if="addressData.name"
		@click="Router.navigateTo({route:{path:'/pages/address/address'}})">
			<image src="../../static/images/thaorder-icon.png" mode="widthFix"></image>
			<view class="py-3 px-2">{{addressData.name?addressData.name:''}} {{addressData.phone?addressData.phone:''}}</view>
			<view class="px-2">{{addressData.city+addressData.detail}}</view>
		</view>
		<view class="pt-4 py-3 line-h" v-else
		@click="Router.navigateTo({route:{path:'/pages/address/address'}})">
			<image src="../../static/images/thaorder-icon.png" mode="widthFix"></image>
			<view class="py-3 px-2">请选择收货地址</view>
		</view>
		<view class="f5Bj-H20"></view>
		
		<view class="px-2 py-3 flex1">
			<image :src="mainData.mainImg&&mainData.mainImg[0]?mainData.mainImg[0].url:''" class="wh180 radius10"></image>
			<view class="h180 flex5 pl-2 flex-1 font-w">
				<view class="avoidOverflow2">{{mainData.title}}</view>
				<view class="font-20 colorR pt-2">
					<text class="font-30 font-w">{{mainData.score}}</text>积分
					<text class="font-30 font-w">+{{mainData.price?mainData.price:''}}</text>元
				</view>
			</view>
		</view>
		<view class="f5Bj-H20"></view>

		
		
		<view style="height: 140rpx;"></view>
		<view class="flex1  p-fX bottom-0 bg-white shadowM">
			<view class="pl-3 flex">合计
				<view class="font-20 colorR pl-1">
					<text class="font-30 font-w">{{mainData.score}}</text>积分
					<text class="font-30 font-w">+{{mainData.price?mainData.price:''}}</text>元
				</view>
			</view>
			
			<view class="btn80 Mgb"
			@click="Utils.stopMultiClick(submit)">确认兑换</view>
		</view>
		
		
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				mainData:{},
				addressData:{},
				Utils:this.$Utils
			}
		},
		
		onLoad(options) {
			const self = this;
			self.id = options.id;
			self.$Utils.loadAll(['getMainData','getUserInfoData'], self);
		},
		onShow() {
			const self = this;
			if(uni.getStorageSync('choosedAddressData')){
				self.addressData = uni.getStorageSync('choosedAddressData')
			}else{
				self.getAddressData()
			};
		},
		
		methods: {
			
			getUserInfoData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.userInfoData = res.info.data[0]
					}
					self.$Utils.finishFunc('getUserInfoData');
				};
				self.$apis.userInfoGet(postData, callback);
			},
			
			getAddressData() {
				const self = this;		
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					isdefault:1
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.addressData = res.info.data[0];
					}
				};
				self.$apis.addressGet(postData, callback);
			},
			
			submit(){
				const self = this;
				uni.setStorageSync('canClick', false);
				
				if(JSON.stringify(self.addressData) == '{}'){
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('请选择收货地址','none')
					return
				}
				if(parseFloat(self.mainData.score)>parseFloat(self.userInfoData.score)){
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('积分不足','none')
					return
				};
				var orderList = [
					{product_id:self.mainData.id,
					count:1}
				]
				self.addOrder(orderList)
			},
			
			addOrder(orderList) {
				const self = this;	
				
				/* if(self.orderId){
					self.goPay()
					return
				}; */
				const postData = {}; 
				postData.orderList = self.$Utils.cloneForm(orderList);
				postData.data = {
					price:parseFloat(parseFloat(self.mainData.score)+parseFloat(self.mainData.price)).toFixed(2),
					type:2,
					snap_address:self.addressData
				};
				postData.tokenFuncName = 'getProjectToken';
				const callback = (res) => {
					uni.setStorageSync('canClick', true);
					if (res && res.solely_code == 100000) {
						self.orderId = res.info.id;
						self.goPay()
					} else {		
						uni.showToast({
							title: res.msg,
							duration: 2000
						});
					};		
				};
				self.$apis.addOrder(postData, callback);
			},
			
			goPay() {
				const self = this;	
				const postData = {
					wxPay:{
						price:parseFloat(self.mainData.price).toFixed(2)
					},
					score:{
						price:parseFloat(self.mainData.score).toFixed(2)
					}
				};	
				postData.tokenFuncName = 'getProjectToken',
				postData.searchItem = {
					id: self.orderId
				};
				const callback = (res) => {
					if (res.solely_code == 100000) {
						uni.setStorageSync('canClick', true);
					
						if (res.info) {
							const payCallback = (payData) => {
								console.log('payData', payData)
								if (payData == 1) {
									uni.showToast({
										title: '兑换成功',
										duration: 1000,
										success: function() {
											
										}
									});
									setTimeout(function() {
										self.$Router.redirectTo({route:{path:'/pages/user-integralOrder/user-integralOrder'}})
									}, 1000);
								} else {
									uni.setStorageSync('canClick', true);
									uni.showToast({
										title: '支付失败',
										duration: 2000
									});
								};
							};
							self.$Utils.realPay(res.info, payCallback);
						} else {
							
							uni.showToast({
								title: '兑换成功',
								duration: 1000,
								success: function() {
									
								}
							});
							setTimeout(function() {
								self.$Router.redirectTo({route:{path:'/pages/user-integralOrder/user-integralOrder'}})
							}, 1000);
						};
					} else {
						uni.setStorageSync('canClick', true);
						uni.showToast({
							title: res.msg,
							duration: 2000
						});
					};
				};
				self.$apis.pay(postData, callback);
			},
			
			getMainData() {
				var self = this;
				var postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
					id:self.id
				};
				var callback = function(res) {
					if (res.info.data.length > 0) {
						self.mainData  = res.info.data[0]
					};
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.productGet(postData, callback);
			},
			
		}
	}
</script>

<style scoped>
.btn80{color: #fff;border-radius: 0;width: 280rpx;line-height: 100rpx;}
.maskCon{width: 600rpx;margin-top: 25%;}
</style>
