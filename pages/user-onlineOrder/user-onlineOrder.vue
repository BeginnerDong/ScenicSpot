<template>
	<view>
		
		<view class="list mb-2 shadowM">
			<view class="li" :class="liCurr==1?'on':''" @click="changeLi(1)">全部</view>
			<view class="li" :class="liCurr==2?'on':''" @click="changeLi(2)">待发车</view>
			<view class="li" :class="liCurr==3?'on':''" @click="changeLi(3)">进行中</view>
			<view class="li" :class="liCurr==4?'on':''" @click="changeLi(4)">已完成</view>
		</view>
		<view v-for="(item,index) in mainData" :key="index">
			<view class="bg-white radius10 mx-2 px-2 mb-2"
			v-for="(c_item,c_index) in item.child"
			:key="c_index" >
				<view class="font-24 color8 flex1 py-2">
					<view>交易时间：{{item.create_time}}</view>
					<view class="colorR" v-if="item.transport_status==0&&item.order_step==0">待发车</view>
					<view class="colorR" v-if="item.transport_status==1&&item.order_step==0">进行中</view>
					<view class="colorR" v-if="item.transport_status==2&&item.order_step==0">已完成</view>
					<view class="colorR" v-if="item.order_step==1">申请退款中</view>
					<view class="colorR" v-if="item.order_step==2">已退款</view>
				</view>
				<view class="flex1 mb-2" :data-id = "item.id"
			@click="Router.navigateTo({route:{path:'/pages/onlineOrder-detail/onlineOrder-detail?id='+$event.currentTarget.dataset.id}})">
					<image :src="c_item.orderItem&&c_item.orderItem[0]&&c_item.orderItem[0].snap_product&&c_item.orderItem[0].snap_product.product&&
								c_item.orderItem[0].snap_product.product.mainImg&&c_item.orderItem[0].snap_product.product.mainImg[0]?c_item.orderItem[0].snap_product.product.mainImg[0].url:''" class="wh180 radius10"></image>
					<view class="h180 flex5 pl-2 flex-1">
						<view class="avoidOverflow2 font-w">{{c_item.product_title?c_item.product_title:''}}</view>
						<view class="flex1">
							<view class="price1 font-34 font-w">{{item.price?item.price:''}}</view>
							<view>{{c_item.count?c_item.count:''}}人</view>
						</view>
					</view>
				</view>
				<view class="flex1 py-3">
					<view>套餐：</view>
					<view>{{item.discount==1?'整车':'拼车'}}套餐（{{c_item.sku_title?c_item.sku_title:''}}） x{{c_item.count?c_item.count:''}}</view>
				</view>
				<view class="d-flex j-end pb-3">
					<view class="btnOrder order2" v-if="item.order_step==0&&item.transport_status==0" @click="orderUpdate(index)">申请退款</view>
					
				</view>
			</view>
			
		</view>
		
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				liCurr:1,
				mainData:[],
				searchItem:{
					level:1,
					type:1,
					pay_status:1
				},
				Router:this.$Router
			}
		},
		onLoad() {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.$Utils.loadAll(['getMainData'], self);
		},
		onReachBottom() {
			console.log('onReachBottom')
			const self = this;
			if (!self.isLoadAll && uni.getStorageSync('loadAllArray')) {
				self.paginate.currentPage++;
				self.getMainData()
			};
		},
		methods: {
			
			orderUpdate(index) {
				const self = this;
				uni.setStorageSync('canClick', false);
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.data = {
					order_step:1
				};
				postData.searchItem = {
					id:self.mainData[index].id,
				};
				const callback = (data) => {
					uni.setStorageSync('canClick', true);
					if (data && data.solely_code == 100000) {
						self.$Utils.showToast('申请成功','none');
						setTimeout(function() {
							self.getMainData(true)
						}, 1000);
					} else {
						self.$Utils.showToast(data.msg,'none')
					}
				};
				self.$apis.orderUpdate(postData, callback);
			 },
			
			changeLi(i){
				const self = this;
				if(i!=self.liCurr){
					self.liCurr = i;
					if(self.liCurr==1){
						delete self.searchItem.transport_status
					}else if(self.liCurr==2){
						self.searchItem.transport_status=0
					}else if(self.liCurr==3){
						self.searchItem.transport_status=1
					}else if(self.liCurr==4){
						self.searchItem.transport_status=2
					};
					self.getMainData(true)
				}
			},
			
			getMainData(isNew) {
				const self = this;
				if (isNew) {
					self.mainData = [];
					self.paginate = {
						count: 0,
						currentPage: 1,
						is_page: true,
						pagesize: 10
					}
				};
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = self.$Utils.cloneForm(self.searchItem);
				postData.searchItem.user_no = uni.getStorageSync('user_info').user_no;
				/* if(self.orderLiCurr==2){
					postData.getAfter = {
						log:{
							tableName:'Log',
							middleKey:'id',
							key:'relation_id',
							searchItem:{
								status:1,
							},
							condition:'='
						}
					};
				} */
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
					}
					console.log('self.mainData', self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.orderGet(postData, callback);
			},
		}
	}
</script>
<style>page{background-color: #f5f5f5;}</style>
<style scoped>
.li{width: 33.33%;}
.btnOrder{width: 130rpx;margin-left: 20rpx;border: 1px solid;text-align: center;border-radius: 10rpx;}
.order2{border-color: #13C3F6;color: #13C3F6;}
</style>
