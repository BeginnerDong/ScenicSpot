<template>
	<view class="pb-5">
		
		<view class="p-r p-2">
			<view class="Mgb p-aX top-0" style="height: 240rpx;"></view>
			<view class="p-r font-30 colorf line-h flex">
				<image :src="userData.headImgUrl?userData.headImgUrl:''" class="wh110 my-2" style="border-radius: 50%;overflow: hidden;"></image>
				<view class="pl-2">
					<view class="pb-3 font-w">{{userData.nickname?userData.nickname:''}}</view>
					<view class="sign" v-if="type==4">代理</view>
					<view class="sign" v-if="type==3">景点</view>
					<view class="sign" v-if="type==1">商户</view>
				</view>
				<view class="font-26 p-a top-0 right-0" @click="loginOff">退出</view>
			</view>
		</view>
		
		<view class="bg-white radius10 font-24 line-h p-r mx-2 shadow">
			<view class="flex4 py-3 p-r">
				<view class="font-40 font-w pb-2">{{allCount}}</view>
				<view class="color6">分润额(元)</view>
			</view>
		</view>
		
		<view class="flex line-h bg-white bB-f5 mt-1">
			<view class="w-50">
				<view class="flex0 w-100 py-3">
					<view>开始时间</view>
					<image src="../../static/images/tehdriver-icon.png" class="xjt-ixon ml-1"></image>
				</view>
				<view class="flex0 w-100 mb-2">
					<dyDatePicker placeholder="请选择" :childValue="start" :minSelect="from_minSelect" :maxSelect="from_maxSelect"
					:iconshow="false" @getData="changeStartTime"></dyDatePicker>
				</view>
				
			</view>
			
			<view class="w-50">
				<view class="flex0 w-100 py-3">
					<view>结束时间</view>
					<image src="../../static/images/tehdriver-icon.png" class="xjt-ixon ml-1"></image>
				</view>
				<view class="flex0 w-100 mb-2">
					<dyDatePicker placeholder="请选择" :childValue="end" :minSelect="to_minSelect" :maxSelect="to_maxSelect"
					:iconshow="false" @getData="changeEndTime"></dyDatePicker>
				</view>
				
			</view>
		</view>
		
		<view class="px-2 line-h bg-white" v-for="(item,index) in mainData" :key="index">
			<view class="flex1 py-4 bB-f5">
				<view class="avoidOverflow pr-5 flex-1">{{item.trade_info}}</view>
				<view class="colorR pl-5">{{item.count}}</view>
			</view>
		</view>
		
		
	</view>
</template>

<script>
	import dyDatePicker from '@/components/dy-Date/dy-Date.vue'
	export default {
		components: {
			dyDatePicker
		},
		data() {
			return {
				Router:this.$Router,
				userData:{},
				type:'',
				searchItem:{
					type:2,
					thirdapp_id:2
				},
				mainData:[],
				end:'',
				start:'',
				from_minSelect: '1900/01/01',
				from_maxSelect: '2050/12/31',
				to_minSelect: '1900/01/01',
				to_maxSelect: '2050/12/31',
				allCount:0
			}
		},
		onLoad(options) {
			const self = this;
			self.type = options.type;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.$Utils.loadAll(['getUserData','getMainData'], self);
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
			
			changeStartTime(e){
				const self = this;
				self.start = e;
				
				self.startTimestamp = self.$Utils.timeToTimestamp(self.start);
				if(self.startTimestamp&&self.endTimestamp){
					self.searchItem.create_time = ['between',[self.startTimestamp,self.endTimestamp]];
					self.getMainData(true)
				};
			},
			
			changeEndTime(e){
				const self = this;
				self.end = e;
				
				self.endTimestamp = self.$Utils.timeToTimestamp(self.end);
				if(self.startTimestamp&&self.endTimestamp){
					self.searchItem.create_time = ['between',[self.startTimestamp,self.endTimestamp]];
					self.getMainData(true)
				};
			},
			
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
				if(self.type==1){
					postData.tokenFuncName = 'getShopToken'
				}else if(self.type==3){
					postData.tokenFuncName = 'getAreaToken'
				}else if(self.type==4){
					postData.tokenFuncName = 'getAgentToken'
				};
				postData.compute = {
				  all:[
					'sum',
					'count',
					self.$Utils.cloneForm(self.searchItem)
				  ],
				};
				if(self.type==1){
					postData.compute.all[2].user_no = uni.getStorageSync('shop_info').user_no
				}else if(self.type==3){
					postData.compute.all[2].user_no = uni.getStorageSync('area_info').user_no
				}else if(self.type==4){
					postData.compute.all[2].user_no = uni.getStorageSync('agent_info').user_no
				};
				
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
					}
					self.allCount = res.info.compute.all
					console.log(self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.flowLogGet(postData, callback);
			},
			
			loginOff(){
				const self = this;
				if(self.type==1){
					uni.removeStorageSync('shop_info');
					uni.removeStorageSync('shop_token');	
				}else if(self.type==3){
					uni.removeStorageSync('area_info');
					uni.removeStorageSync('area_token');	
				}else if(self.type==4){
					uni.removeStorageSync('agent_info');
					uni.removeStorageSync('agent_token');	
				}
				self.Router.redirectTo({route:{path:'/pages/user/user'}})
			},
			
			getUserData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.userData = res.info.data[0];
					};
					self.$Utils.finishFunc('getUserData');
				};
				self.$apis.userGet(postData, callback);
			},
		}
	}
</script>

<style>
page{background-color: #fff;}
</style>
