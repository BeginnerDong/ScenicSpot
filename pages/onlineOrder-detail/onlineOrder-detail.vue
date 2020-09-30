<template>
	<view class="px-2">
		
		<view class="px-2 py-3 flex1 bg-white radius10 mt-2" v-for="(c_item,c_index) in mainData.child"
			:key="c_index">
			<image :src="c_item.orderItem&&c_item.orderItem[0]&&c_item.orderItem[0].snap_product&&c_item.orderItem[0].snap_product.product&&
			c_item.orderItem[0].snap_product.product.mainImg&&c_item.orderItem[0].snap_product.product.mainImg[0]?c_item.orderItem[0].snap_product.product.mainImg[0].url:''" class="wh180 radius10"></image>
			<view class="h180 flex5 pl-2 flex-1 font-w">
				<view class="avoidOverflow2">{{c_item.product_title?c_item.product_title:''}}</view>
				<view class="flex1">
					<view class="price1 font-34 font-w">{{mainData.price?mainData.price:''}}</view>
					<view>{{mainData.standard?mainData.standard:''}}人</view>
				</view>
			</view>
		</view>
		
		<view class="bg-white mt-3  px-2 radius10 line-h">
			<view class="pt-3 pb-4 flex1 bB-de1">
				<view class="font-w">{{mainData.discount==1?'整车':'拼车'}}套餐（{{mainData.child&&mainData.child[0]?mainData.child[0].sku_title:''}}）</view>
				<view class="color9 font-26">x{{mainData.child&&mainData.child[0]?mainData.child[0].count:''}}</view>
			</view>
			<view class="bB-de1 pt-4">
				<view class="pb-4 flex1">
					<textarea v-model="mainData.child[0].snap_product.description"></textarea>
				</view>
			</view>
			<view class="py-3 flex1">
				<view>总计：</view>
				<text class="price">{{mainData.price?mainData.price:''}}</text>
			</view>
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
