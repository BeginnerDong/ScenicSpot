<template>
	<view>
		
		<image src="../../static/images/commission-icon.png" mode="widthFix" class="p-fX top-0"></image>
		<view class="colorf p-r wh m-a mt-5">
			<image src="../../static/images/commission-icon1.png" mode="widthFix"></image>
			<view class="p-aXY text-center pt-4">
				<view class="font-80 bB-f d-inline-block">{{mainData.price?mainData.price:0}}</view>
				<view class="font-22 pt-1">押金</view>
			</view>
		</view>
		
		<view class="btn80 colorM" v-if="mainData.price&&mainData.order_step==0"
		@click="orderUpdate">申请退还</view>
		
		<view class="btn80 colorM" v-if="mainData.price&&mainData.order_step==1">已申请退还</view>
		
		<view class="btn80 colorM" v-if="!mainData.price">暂未缴纳押金</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				mainData:{}
			}
		},
		onLoad() {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			getMainData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					type:6,
					pay_status:1
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData = res.info.data[0]
					}
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.orderGet(postData, callback);
			},
			orderUpdate() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					id:self.mainData.id
				};
				postData.data = {
					order_step:1
				};
				const callback = (res) => {
					if (res.solely_code==100000) {
						self.$Utils.showToast('申请成功');
						setTimeout(function() {
							self.getMainData()
						}, 1000);
					}
				};
				self.$apis.orderUpdate(postData, callback);
			},
		}
	}
</script>

<style scoped>
.wh{width: 225rpx;height: 237rpx;}
.bB-f{border-bottom: 1px solid #fff;}
.btn80{margin: 250rpx 20rpx 0;background-color: #fff;border-radius: 10rpx;border: 1px solid #13C3F6;}
</style>
