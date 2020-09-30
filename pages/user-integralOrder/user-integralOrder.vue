<template>
	<view>
		
		<view class="list mb-2 shadowM">
			<view class="li" :class="liCurr==1?'on':''" @click="changeLi(1)">全部</view>
			<view class="li" :class="liCurr==2?'on':''" @click="changeLi(2)">待发货</view>
			<view class="li" :class="liCurr==3?'on':''" @click="changeLi(3)">待收货</view>
			<view class="li" :class="liCurr==4?'on':''" @click="changeLi(4)">已收货</view>
		</view>
		<view  v-for="(item,index) in mainData" :key="index">
			<view class="bg-white radius10 mx-2 px-2 mb-2"
			v-for="(c_item,c_index) in item.child"
			:key="c_index" 
			>
				<view class="font-24 color8 flex1 py-2">
					<view>交易时间：{{item.create_time}}</view>
					<view class="colorR" v-if="item.transport_status==0">待发货</view>
					<view class="colorR" v-if="item.transport_status==1">待收货</view>
					<view class="colorR" v-if="item.transport_status==2">已收货</view>
				</view>
				<view class="flex1"
				:data-id = "item.id"
				@click="Router.navigateTo({route:{path:'/pages/orderDetail/orderDetail?id='+$event.currentTarget.dataset.id}})">
					<image :src="c_item.orderItem&&c_item.orderItem[0]&&c_item.orderItem[0].snap_product&&c_item.orderItem[0].snap_product&&
								c_item.orderItem[0].snap_product.mainImg&&c_item.orderItem[0].snap_product.mainImg[0]?c_item.orderItem[0].snap_product.mainImg[0].url:''" class="wh180 radius10"></image>
					<view class="h180 flex5 pl-2 flex-1">
						<view class="avoidOverflow2 font-w">{{c_item.product_title?c_item.product_title:''}}</view>
						<view class="flex1">
							<view class="font-20 colorR pt-2">
								<text class="font-30 font-w">{{c_item.orderItem&&c_item.orderItem[0]&&c_item.orderItem[0].snap_product&&c_item.orderItem[0].snap_product?
								c_item.orderItem[0].snap_product.score:''}}</text>积分
								<text class="font-30 font-w">+{{item.price?item.price:''}}</text>元
							</view>
							<view>x{{c_item.count?c_item.count:''}}</view>
						</view>
					</view>
				</view>
				<view class="py-2 text-right">
					<view class="btn text-center font-26 color8 b-e1 radius10" v-if="item.transport_status==1">确认收货</view>
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
					type:2,
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
.li{width: 25%;}
.btn{width: 150rpx;line-height: 60rpx;display: inline-block;}
</style>
