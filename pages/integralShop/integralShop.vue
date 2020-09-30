<template>
	<view>
		
		<view class="px-2 pb-2 flex1">
			<view class="w345 radius10 overflow-h p-r bg-white mt-2"
			:data-id = "item.id"
			 v-for="(item,index) in mainData" :key="index"
			@click="Router.navigateTo({route:{path:'/pages/integralDetail/integralDetail?id='+$event.currentTarget.dataset.id}})">
				<image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" class="img"></image>
				<view class="kcSign">库存 {{item.stock}}</view>
				<view class="p-2">
					<view class="avoidOverflow2">{{item.title?item.title:''}}</view>
					<view class="font-20 colorR pt-2">
						<text class="font-30 font-w">{{item.score?item.score:''}}</text>积分
						<text class="font-30 font-w">+{{item.price?item.price:''}}</text>元
					</view>
				</view>
			</view>
		</view>
		
		
		
		<view style="height: 130rpx;"></view>
		<!-- footer -->
		<view class="footer">
			<view class="item" @click="Router.redirectTo({route:{path:'/pages/index/index'}})">
				<image src="../../static/images/nabar1.png" mode=""></image>
				<view>首页</view>
			</view>
			<view class="item on">
				<image src="../../static/images/nabar2-a.png" mode=""></image>
				<view>积分商城</view>
			</view>
			<view class="item" @click="Router.redirectTo({route:{path:'/pages/user/user'}})">
				<image src="../../static/images/nabar3.png" mode=""></image>
				<view>我的</view>
			</view>
		</view>
		
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				mainData:[]
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
			
			getMainData(isNew) {
				var self = this;
				if (isNew) {
					self.mainData = [];
					self.paginate = {
						count: 0,
						currentPage: 1,
						pagesize: 10,
						is_page: true,
					}
				};
				var postData = {};
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.noLoading = true;
				postData.searchItem = {
					thirdapp_id: 2,
					type:2
				};
				postData.order = {
					listorder: 'desc'
				};
				var callback = function(res) {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
					};
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.productGet(postData, callback);
			},
			
		}
	}
</script>
<style>page{background-color: #f5f5f5;}</style>
<style scoped>
.w345{width: 345rpx;}
.img{width: 345rpx;height: 345rpx;}
.kcSign{line-height: 40rpx;padding: 0 10rpx;background-color: rgba(0,0,0,0.5);color: #fff;font-size: 24rpx;border-radius: 10rpx 0 10rpx 0;position: absolute;top: 0;left: 0;}
</style>
