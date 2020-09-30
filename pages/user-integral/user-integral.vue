<template>
	<view>
		
		<image src="../../static/images/commission-icon.png" mode="widthFix" class="p-fX top-0"></image>
		<view class="colorf p-r wh m-a top">
			<image src="../../static/images/commission-icon1.png" mode="widthFix"></image>
			<view class="p-aXY text-center pt-4">
				<view class="font-80 bB-f d-inline-block">{{userInfoData.score?userInfoData.score:0}}</view>
				<view class="font-22 pt-1">可用积分</view>
			</view>
		</view>
		
		<view class="line-h px-2 py-3 bB-f5 flex1" v-for="(item,index) in mainData" :key="index">
			<view>
				<view>{{item.trade_info}}</view>
				<view class="font-26 color8 pt-3">{{item.create_time}}</view>
			</view>
			<view class="font-30 colorR">{{item.count}}</view>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				userInfoData:{},
				mainData:[],
				searchItem:{
					thirdapp_id:2,
					type:3,
					count:['not in',[0]]
				}
			}
		},
		onLoad() {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.$Utils.loadAll(['getMainData','getUserInfoData'], self);
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
			
			getUserInfoData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.userInfoData = res.info.data[0]
						self.userInfoData.score = parseInt(self.userInfoData.score)
					}
					self.$Utils.finishFunc('getUserInfoData');
				};
				self.$apis.userInfoGet(postData, callback);
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
					}
				};
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = self.$Utils.cloneForm(self.searchItem);
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
						/* for (var i = 0; i < self.mainData.length; i++) {
							self.mainData[i].score = parseInt(self.mainData[i].score)
						} */
					}
					self.$Utils.finishFunc('getMainData');
			
				};
				self.$apis.flowLogGet(postData, callback);
			},
			
		}
	}
</script>

<style scoped>
.top{margin-top: 60rpx;margin-bottom: 90rpx;}
.wh{width: 225rpx;height: 237rpx;}
.bB-f{border-bottom: 1px solid #fff;}
</style>
