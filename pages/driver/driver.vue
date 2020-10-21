<template>
	<view class="pb-5">
		
		<view class="p-r p-2">
			<view class="Mgb p-aX top-0" style="height: 240rpx;"></view>
			<view class="p-r font-30 colorf line-h flex">
				<image :src="nameData.headImgUrl?nameData.headImgUrl:''" class="wh110 my-2" style="border-radius: 50%;overflow: hidden;"></image>
				<view class="pl-2">
					<view class="pb-3 font-w">{{nameData.nickname?nameData.nickname:''}}</view>
					<view class="sign">司机</view>
				</view>
				<view class="font-26 p-a top-0 right-0"  @click="loginOff">退出</view>
			</view>
		</view>
		
		<view class="bg-white radius10 flex1 font-24 line-h p-r mx-2 mb-3">
			<view class="w33 flex4 py-3 p-r">
				<view class="font-40 font-w pb-2">{{userData.order&&userData.order.all?userData.order.all:0}}</view>
				<view class="color6">订单总数</view>
			</view>
			<view class="w33 flex4 py-3 p-r"
			@click="Router.navigateTo({route:{path:'/pages/driverProfit/driverProfit'}})">
				<view class="font-40 font-w pb-2">{{userData.balance&&userData.balance.all?userData.balance.all:0}}</view>
				<view class="color6">收益</view>
			</view>
			<view class="w33 flex4 py-3 p-r">
				<view class="font-40 font-w pb-2">{{userData.order&&userData.order.today?userData.order.today:0}}</view>
				<view class="color6">今日订单数</view>
			</view>
		</view>
		
		<view class="bg-white pt-3 shadow">
			<view class="mx-2 radius10 borderM overflow-h h70 text-center flex">
				<view class="w-50" :class="navCurr==0?'Mgb colorf':''" @click="change('nav',0)">整车订单</view>
				<view class="w-50" :class="navCurr==1?'Mgb colorf':''" @click="change('nav',1)">拼车订单</view>
			</view>
			<view class="list">
				<view class="li" :class="liCurr==0?'on':''" @click="change('li',0)">全部</view>
				<view class="li" :class="liCurr==1?'on':''" @click="change('li',1)">待发车</view>
				<view class="li" :class="liCurr==2?'on':''" @click="change('li',2)">进行中</view>
				<view class="li" :class="liCurr==3?'on':''" @click="change('li',3)">已完成</view>
			</view>
		</view>
		
		<view class="bg-white radius10 mx-2 px-2 mt-3" v-show="navCurr==0" v-for="(item,index) in mainData"
		:key="index">
			<view class="font-24 color8 flex1 py-2">
				<view>交易时间：{{item.create_time}}</view>
				<view class="colorR" v-if="item.transport_status==0">待发车</view>
				<view class="colorR" v-if="item.transport_status==1">进行中</view>
				<view class="colorR" v-if="item.transport_status==2">已完成</view>
			</view>
			<view v-for="(c_item,c_index) in item.child"
			:key="c_index">
				<view class="flex1 pb-2 bB-df5">
					<image :src="c_item.orderItem&&c_item.orderItem[0]&&c_item.orderItem[0].snap_product&&c_item.orderItem[0].snap_product.product&&
								c_item.orderItem[0].snap_product.product.mainImg&&c_item.orderItem[0].snap_product.product.mainImg[0]?c_item.orderItem[0].snap_product.product.mainImg[0].url:''" class="wh180 radius10"></image>
					<view class="h180 flex5 pl-2 flex-1">
						<view class="avoidOverflow2 font-w">{{c_item.product_title?c_item.product_title:''}}</view>
						<view class="flex1">
							<view class="price1 font-34 font-w">{{item.price?item.price:''}}</view>
							<view>{{item.standard?item.standard:''}}人</view>
						</view>
					</view>
				</view>
				<view class="flex1 py-3 bB-df5">
					<view>套餐：</view>
					<view>{{item.discount==1?'整车':'拼车'}}套餐（{{c_item.sku_title?c_item.sku_title:''}}） x{{c_item.count?c_item.count:''}}</view>
				</view>
				<view class="flex1 py-2 bB-df5">
					<view>二维码：</view>
					<image :src="item.qrCode" class="wh80"></image>
				</view>
			</view>
			
			<view class="py-2 text-right" v-if="item.transport_status==0" @click="orderUpdate(1,index)">
				<view class="btn text-center font-26 b-e1 radius10">确认发车</view>
			</view>
			<view class="py-2 text-right" v-if="item.transport_status==1" @click="orderUpdate(2,index)">
				<view class="btn text-center font-26 b-e1 radius10">确认完成</view>
			</view>
		</view>
		
		
		
		<view class="bg-white radius10 mx-2 px-2 mt-3" v-show="navCurr==1" v-for="(item,index) in mainData"
		:key="index"
		>
			<view class="font-24 color8 flex1 py-2">
				<view>交易时间：{{item.create_time}}</view>
				<view class="colorR" v-if="item.transport_status==0">待发车</view>
				<view class="colorR" v-if="item.transport_status==1">进行中</view>
				<view class="colorR" v-if="item.transport_status==2">已完成</view>
			</view>
			<view v-for="(c_item,c_index) in item.child"
			:key="c_index">
				<view class="flex1 pb-2 bB-df5" 
				@click="Router.navigateTo({route:{path:'/pages/onlineOrder-detail/onlineOrder-detail'}})">
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
				<view class="flex1 py-3 bB-df5">
					<view>套餐：</view>
					<view>{{item.discount==1?'整车':'拼车'}}套餐（{{c_item.sku_title?c_item.sku_title:''}}） x{{c_item.count?c_item.count:''}}</view>
				</view>
				<view class="flex1 py-2 bB-df5">
					<view>二维码：</view>
					<image :src="item.qrCode" class="wh80"></image>
				</view>
			</view>
			
			<view class="py-2 text-right" v-if="item.transport_status==0&&liCurr!=0" @click="allOrderUpdate(1)">
				<view class="btn text-center font-26 b-e1 radius10">确认发车</view>
			</view>
			<view class="py-2 text-right" v-if="item.transport_status==1&&liCurr!=0" @click="allOrderUpdate(2)">
				<view class="btn text-center font-26 b-e1 radius10">确认完成</view>
			</view>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				liCurr:0,
				navCurr:0,
				userData:{},
				nameData:{},
				searchItem:{
					pay_status:1,
					user_type:0,
					discount:1
				},
				mainData:[]
			}
		},
		onLoad(options) {
			const self = this;
			self.type = options.type;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.$Utils.loadAll(['getUserData','getNameData','getMainData'], self);
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
			getMainData(isNew) {
				const self = this;
				if (isNew) {
					self.mainData = [];
					self.paginate = {
						count: 0,
						currentPage: 1,
						pagesize: 10,
						is_page: true,
					};
				};
				const postData = {};
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = self.$Utils.cloneForm(self.searchItem);
				postData.tokenFuncName = 'getDriverToken';
				postData.searchItem.driver_no = uni.getStorageSync('driver_info').user_no;
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
					}
					console.log(self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.orderGet(postData, callback);
			},
			loginOff(){
				const self = this;
				uni.removeStorageSync('driver_info');
				uni.removeStorageSync('driver_token');
				self.Router.redirectTo({route:{path:'/pages/user/user'}})
			},
			getNameData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.nameData = res.info.data[0];
					};
					self.$Utils.finishFunc('getNameData');
				};
				self.$apis.userGet(postData, callback);
			},
			orderUpdate(num,index) {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getDriverToken';
				postData.searchItem = {
					id:self.mainData[index].id,
					user_type:0
				};
				postData.data = {
					transport_status:num
				};	
				postData.saveAfter = [
					{
						tableName: 'UserInfo',
						FuncName: 'update',
						data: {
							drive_time:(new Date()).getTime() / 1000,
							drive_status:1
						},
						searchItem:{
							user_no:uni.getStorageSync('driver_info').user_no
						}
					}
				];	
				if(postData.data.transport_status==2){
					postData.saveAfter[0].data.drive_status=0
				}
				const callback = (res) => {
					if (res.solely_code==100000) {
						self.$Utils.showToast('操作成功', 'none');
						setTimeout(function() {
							self.getMainData(true)
						}, 1000);
					};
				};
				self.$apis.orderUpdate(postData, callback);
			},
			allOrderUpdate(num) {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getDriverToken';
				postData.searchItem = {
					pay_status:1,
					user_type:0,
					discount:2,
				};
				postData.searchItem.driver_no = uni.getStorageSync('driver_info').user_no;
				postData.data = {
					transport_status:num
				};	
				postData.saveAfter = [
					{
						tableName: 'UserInfo',
						FuncName: 'update',
						data: {
							drive_time:(new Date()).getTime() / 1000,
							drive_status:1
						},
						searchItem:{
							user_no:uni.getStorageSync('driver_info').user_no
						}
					}
				];	
				if(postData.data.transport_status==2){
					postData.saveAfter[0].data.drive_status=0
				}
				const callback = (res) => {
					if (res.solely_code == 100000) {
						self.$Utils.showToast('操作成功', 'none');
						setTimeout(function() {
							self.getMainData(true)
						}, 1000);
					};
				};
				self.$apis.orderUpdate(postData, callback);
			},
			getUserData() {
				const self = this;
				var dayStart = new Date(new Date().setHours(0, 0, 0, 0)).getTime() / 1000;
				var nowTime = (new Date()).getTime() / 1000;
				const postData = {};
				postData.tokenFuncName = 'getDriverToken';
				postData.getAfter = {
					order:{
						tableName:'Order',
						middleKey:'user_no',
						key:'driver_no',
						searchItem:{
							status:1,
						},
						condition:'=',
						compute: {
							all: [
								'count',
								'count',
								{
									status: 1,
									//user_no:uni.getStorageSync('driver_info').user_no
								}
							],
							today: [
								'count',
								'count',
								{
									status: 1,
									//user_no:uni.getStorageSync('driver_info').user_no,
									create_time:['between',[dayStart,nowTime]]
								}
							],
						}
					},
					balance:{
						tableName:'FlowLog',
						middleKey:'user_no',
						key:'user_no',
						searchItem:{
							status:1,
							type:2
						},
						condition:'=',
						compute: {
							all: [
								'sum',
								'count',
								{
									status: 1,
									user_no:uni.getStorageSync('driver_info').user_no,
									type:2
								}
							],
						}
					},
				}
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.userData = res.info.data[0];
					};
					self.$Utils.finishFunc('getUserData');
				};
				self.$apis.userGet(postData, callback);
			},
			change(type,i){
				const self = this;
				if(type == 'nav'){
					if(self.navCurr!=i){
						self.navCurr =i;
						if(self.navCurr==0){
							self.searchItem.discount = 1
						}else if(self.navCurr==1){
							self.searchItem.discount = 2
						}
						self.getMainData(true)
					}
				}else{
					if(i!=self.liCurr){
						self.liCurr = i;
						if(self.liCurr==0){
							delete self.searchItem.transport_status
						}else if(self.liCurr==1){
							self.searchItem.transport_status=0
						}else if(self.liCurr==2){
							self.searchItem.transport_status=1
						}else if(self.liCurr==3){
							self.searchItem.transport_status=2
						};
						self.getMainData(true)
					}
				}
			}
			
		}
	}
</script>

<style>
page{background-color: #f5f5f5;}
</style>
<style scoped>

.w33,.li{width: 33.33%;}
.w33::before{content: '';height: 40rpx;border-left: 1px solid #f5f5f5;position: absolute;right: 0;top: 50%;margin-top: -20rpx;}

.h70{line-height: 70rpx;}

.btn{width: 150rpx;line-height: 60rpx;display: inline-block;}
</style>
