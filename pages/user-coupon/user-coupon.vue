<template>
	<view>
		
		<view class="list mb-3 shadowM">
			<view class="li" :class="liCurr==0?'on':''" @click="changeLi(0)">未使用</view>
			<view class="li" :class="liCurr==1?'on':''" @click="changeLi(1)">已使用</view>
			<view class="li" :class="liCurr==2?'on':''" @click="changeLi(2)">已过期</view>
		</view>
		
		<view class="mx-2 mb-3 p-r" v-for="(item,index) in mainData" :key="index">
			<image src="../../static/images/coupons-icon.png" mode="widthFix" v-if="liCurr==0"></image>
			<image src="../../static/images/coupons-icon1.png" mode="widthFix" v-else></image>
			<view class="p-aXY flex">
				<view class="price1">{{item.value}}</view>
				<view class="txt line-h font-24 flex5 p-r pl-4 py-1" :class="liCurr==0?'b1':'b2'">
					<view class="font-30 font-w">{{item.snap_coupon.behavior==1?'整车':'拼车'}}专享</view>
					<view class="py-3">满{{item.condition}}减{{item.value}}元</view>
					<view>有效期至：{{Utils.timeto(item.invalid_time,'ymd')}}</view>
				</view>
			</view>
			<image src="../../static/images/coupons-icon2.png" class="icon" v-show="liCurr==1"></image>
			<image src="../../static/images/coupons-icon3.png" class="icon" v-show="liCurr==2"></image>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				liCurr:0,
				searchItem:{
					thirdapp_id:2,
					use_step:1,
				},
				mainData:[],
				Utils:this.$Utils
			}
		},
		onLoad() {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.$Utils.loadAll(['getMainData'], self);
		},
		
		onShow(options) {
			const self = this;
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
					if(self.liCurr==0){
						self.searchItem.use_step=1
					}else if(self.liCurr==1){
						self.searchItem.use_step=2
					}else if(self.liCurr==2){
						self.searchItem.use_step=-1
					};
					self.getMainData(true)
				}
			},
			
			
			getMainData(isNew) {
				const self = this;
				if(isNew){
					self.mainData = [];
					self.paginate = {
						count: 0,
						currentPage:1,
						pagesize:10,
						is_page:true,
					}
				};
				const postData = {};
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = self.$Utils.cloneForm(self.searchItem);
				postData.tokenFuncName = 'getProjectToken';
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
						for (var i = 0; i < self.mainData.length; i++) {
							self.mainData[i].invalid_time = parseInt(self.mainData[i].invalid_time)
						}
					} else {
						self.$Utils.showToast('没有更多了', 'none');
					};
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.userCouponGet(postData, callback);
			},
		}
	}
</script>
<style>page{background-color: #f5f5f5;}</style>
<style scoped>
.li{width: 33.33%;}
.price1{font-size: 100rpx;font-weight: bold;color: #FF3D12;width: 200rpx;text-align: center;}
.txt{height: 155rpx;}
.b1{border-left: 1px dashed #13C3F6;}
.b2{border-left: 1px dashed #1888;}
.icon{width: 97rpx;height: 94rpx;position: absolute;top: 0;right: 0;}
</style>
