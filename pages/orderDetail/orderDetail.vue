<template>
	<view class="px-2">
		
		<view class="pb-3 line-h bg-white radius10 mt-2 overflow-h">
			<image src="../../static/images/thaorder-icon.png" mode="widthFix"></image>
			<view class="py-3 px-2">{{mainData.snap_address?mainData.snap_address.name:''}} {{mainData.snap_address?mainData.snap_address.phone:''}}</view>
			<view class="px-2">{{mainData.snap_address.city+mainData.snap_address.detail?mainData.snap_address.city+mainData.snap_address.detail:''}}</view>
		</view>
		
		<view class="px-2 py-3 flex1 bg-white radius10 mt-2" v-for="(item,index) in mainData.child" :key="index">
			<image :src="item.orderItem&&item.orderItem[0]&&item.orderItem[0].snap_product&&item.orderItem[0].snap_product&&
								item.orderItem[0].snap_product.mainImg&&item.orderItem[0].snap_product.mainImg[0]?item.orderItem[0].snap_product.mainImg[0].url:''" class="wh180 radius10"></image>
			<view class="h180 flex5 pl-2 flex-1 font-w">
				<view class="avoidOverflow2">{{item.product_title?item.product_title:''}}</view>
				<view class="font-20 colorR pt-2">
					<text class="font-30 font-w">{{item.orderItem&&item.orderItem[0]&&item.orderItem[0].snap_product&&item.orderItem[0].snap_product?
								item.orderItem[0].snap_product.score:''}}</text>积分
					<text class="font-30 font-w">+{{item.price?item.price:''}}</text>元
				</view>
			</view>
		</view>
		
		<view class="px-2 py-3 bg-white radius10 mt-2 font-24 color8">
			<view class="pb-3">订单编号：{{mainData.order_no?mainData.order_no:''}}</view>
			<view>下单时间：{{mainData.create_time?mainData.create_time:''}}</view>
		</view>
		
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
					id: self.id,
				};
				/* postData.getAfter = {
					shop: {
						tableName: 'UserInfo',
						middleKey: 'shop_no',
						key: 'user_no',
						searchItem: {
							status: 1,
							user_type: 1
						},
						condition: '=',
						info: ['name', 'address','phone']
					}
				}; */
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData = res.info.data[0];
					};
					console.log('self.mainData', self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.orderGet(postData, callback);
			},
			
		}
	}
</script>

<style>
page{background-color: #f5f5f5;}
</style>
