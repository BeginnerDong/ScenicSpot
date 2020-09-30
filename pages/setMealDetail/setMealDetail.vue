<template>
	<view>
		
		<view class="bg-white m-2 mb-0 px-2 radius10 line-h" v-for="(item,index) in mainData" :key="index">
			<view class="pt-3 pb-4 font-34 font-w flex1">
				<view class="tit p-r t-indent20">{{item.title}}</view>
				
				<!-- <image src="../../static/images/detailsonline-icon3.png" class="wh46"></image> -->
				<image @click="choose(index)" :src="chooseId==item.id?'../../static/images/detailsonline-icon3.png':'../../static/images/detailsonline-icon4.png'" class="wh46"></image>
			</view>
			<view class="pb-4 flex1">
				
				<textarea contenteditable="true" auto-height="true" style="background-color: #f5f5f5;padding: 10rpx;" disabled  v-model="item.description"></textarea>
			</view>
			<view class="text-right py-4 bT-f5">
				总计：<text class="price">{{item.price}}</text>
			</view>
		</view>
		
		
		
		<view style="height: 140rpx;"></view>
		<!-- <view class="flex1 p-2 bT-f5 p-fX bottom-0 bg-white">
			<view class="btn80 Mgb"
			@click="submit">确定</view>
		</view> -->
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				chooseId:'',
				mainData:[],
				orderList:[]
			}
		},
		onLoad(options) {
			const self = this;
			self.behavior = options.behavior;
			if(options.id){
				self.chooseId = options.id
			};
			self.$Utils.loadAll(['getMainData'], self);
		},
		onShow() {
			const self = this;
			self.orderList = [];
			self.getUserInfoData();
		},
		methods: {
			
			choose(index){
				const self = this;
				self.chooseSku = self.mainData[index];
				self.chooseId = self.chooseSku.id
			},
			
			getUserInfoData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.noLoading = true;
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.userInfoData = res.info.data[0]
					}
					self.$Utils.finishFunc('getUserInfoData');
				};
				self.$apis.userInfoGet(postData, callback);
			},
			
			submit(behavior){
				const self = this;
				/* if(self.userInfoData.deposit==0){
					uni.showModal({
						title:'提示',
						content:'您还未缴纳押金，是否立即缴纳',
						success(res) {
							if(res.confirm){
								self.Router.navigateTo({route:{path:'/pages/deposit/deposit'}})
							}
						}
					})
					return
				}; */
				if(JSON.stringify(self.chooseSku)!='{}'){
					self.orderList.push(
						{sku_id:self.chooseSku.id,count:1,product:self.productData,sku:self.chooseSku},
					);
					uni.setStorageSync('payPro', self.orderList);
					self.Router.navigateTo({route:{path:'/pages/onlineOrder/onlineOrder'}})
				}else{
					
					self.Router.navigateTo({route:{path:'/pages/setMealDetail/setMealDetail?behavior='+behavior}})
				}
			},
			
			getMainData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					product_no:uni.getStorageSync('productNo'),
					
				};
				postData.getAfter = {
					sku:{
						tableName:'Sku',
						middleKey:'product_no',
						key:'product_no',
						searchItem:{
							status:1,
							behavior:self.behavior
						},
						condition:'='
					}
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData = res.info.data[0].sku;
						self.productData = res.info.data[0];
						self.chooseSku = self.mainData[0];
						if(self.chooseId){
							for (var i = 0; i < self.mainData.length; i++) {
								if(self.mainData[i].id==self.chooseId){
									self.chooseSku = self.mainData[i]
								}
							}
						}
					}
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.productGet(postData, callback);
			},
			
		}
	}
</script>

<style>
page{background: #f5f5f5;}
</style>
<style scoped>
.tit::before{content: '';width: 8rpx;height: 20rpx;background-color: #13C3F6;position: absolute;top: 0;left: 0;}
.btn80{color: #fff;border-radius: 10rpx;width: 100%;}
</style>