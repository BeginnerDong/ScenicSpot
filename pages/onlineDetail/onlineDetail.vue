<template>
	<view>
		
		<!-- banner -->
		<swiper class="swiper-box" indicator-dots="indicatorDots" autoplay="autoplay" interval="3000" indicator-active-color="#fff">
			<block>
				<swiper-item class="swiper-item" v-for="(item,index) in mainData.bannerImg" :key="index">
					<image :src="item.url" mode="widthFix" />
				</swiper-item>
			</block>
		</swiper>
		
		<!-- 详情信息 -->
		<view class="px-2 py-3">
			<view class="font-32">{{mainData.title}}</view>
			<view class="font-24 color8 flex-1 pt-2 pb-3">{{mainData.description}}</view>
			<view class="flex1">
				<view>
					<text class="price1 font-34 font-w">{{chooseSku.price?chooseSku.price:'暂无'}}</text>
					<text class="font-20 color9 pl-2">已售 {{mainData.sale_count?mainData.sale_count:0}}</text>
				</view>
				<view class="font-24 colorM">可得积分 {{chooseSku.score?chooseSku.score:'暂无'}}</view>
			</view>
		</view>
		<view class="f5Bj-H20"></view>
		
		<!-- 拼团 -->
		<view class="px-2 py-3" v-if="mainData.orders&&mainData.orders.length>0">
			<view class="font-22 color8">这些人刚刚购买成功，可参与拼团</view>
			<view class="flex1 pt-3">
				<image :src="mainData.orders&&mainData.orders[0]&&mainData.orders[0].user?mainData.orders[0].user.headImgUrl:''" class="wh80"></image>
				<view class="font-24 pl-2 flex-1">{{mainData.orders&&mainData.orders[0]&&mainData.orders[0].user?mainData.orders[0].user.nickname:''}}</view>
				<view class="font-26">{{mainData.orders&&mainData.orders[0]?mainData.orders[0].count:''}}人</view>
			</view>
		</view>
		<view class="f5Bj-H20"></view>
		
		<!-- 套餐 -->
		<view class="font-24 px-2">
			<view>
				<view class="flex1 py-4">
					<view class="font-34 font-w">整车套餐</view>
					<view class="flex color9" @click="Router.navigateTo({route:{path:'/pages/setMealDetail/setMealDetail?id='+chooseSku.id+'&behavior=1'}})">
						<view>查看详情</view>
						<image src="../../static/images/icon.png" class="R-icon ml-1"></image>
					</view>
				</view>
				<view class="pt-1 bB-e1 flex flex-wrap tcBox">
					<view class="tc flex0 p-r b-e1 radius10 mb-4"  v-for="(item,index) in mainData.carcharter" :key="index" v-if="mainData.carcharter&&mainData.carcharter.length>0" @click="choose(index,'carcharter')" :class="item.id==chooseSku.id?'on':''">
						<view class="font-44 font-w pt-2 price1">{{item.price}}</view>
						<view class="tcSign avoidOverflow">{{item.title}}</view>
					</view>
				</view>
				<view style="width: 100%;text-align: center;margin: 50rpx 0;" v-if="mainData.carcharter&&mainData.carcharter.length==0">
					暂无整车套餐~
				</view>
			</view>
			<view>
				<view class="flex1 py-4">
					<view class="font-34 font-w">拼车套餐</view>
					<view class="flex color9" @click="Router.navigateTo({route:{path:'/pages/setMealDetail/setMealDetail?id='+chooseSku.id+'&behavior=2'}})">
						<view>查看详情</view>
						<image src="../../static/images/icon.png" class="R-icon ml-1"></image>
					</view>
				</view>
				<view class="pt-1 bB-e1 flex flex-wrap tcBox" >
					<view class="tc flex0 p-r b-e1 radius10 mb-4" v-for="(item,index) in mainData.carpool" :key="index"  v-if="mainData.carpool&&mainData.carpool.length>0" :class="item.id==chooseSku.id?'on':''" @click="choose(index,'carpool')">
						<view class="font-44 font-w pt-2 price1">{{item.price}}</view>
						<view class="tcSign avoidOverflow">{{item.title}}</view>
					</view>
				</view>
				<view style="width: 100%;text-align: center;margin: 50rpx 0;" v-if="mainData.carpool&&mainData.carpool.length==0">
					暂无拼车套餐~
				</view>
			</view>
		</view>
		<view class="f5Bj-H20"></view>
		
		<!-- 服务协议 -->
		<view class="font-24 fw d-flex j-center">
			点击支付表示已经同意 
			<text class="colorR"
			@click="Router.navigateTo({route:{path:'/pages/service/service'}})">《服务协议》</text>
			<image @click="select()" style="width: 40rpx;height: 40rpx;margin-left: 20rpx;" :src="isAgree?'../../static/images/address-icon3.png':'../../static/images/address-icon2.png'"></image>
		</view>
		<view class="f5Bj-H20"></view>
		<!-- 旅游行程安排 -->
		<view class="px-2 py-3">
			<view class="font-34 font-w">旅游行程安排</view>
			<view class="py-2">
				<view class="content ql-editor" style="padding:0" v-html="mainData.content">
				</view>
			</view>
		</view>
		
		
		
		
		
		<!-- 按钮 -->
		<!-- <view class="flex1 p-2 bT-f5 p-fX bottom-0 bg-white">
			<view class="btn80 Mgb"
			@click="submit(2)">购买拼车</view>
			<view class="btn80 Mgb"
			@click="submit(1)">购买整车</view>
		</view> -->
		
		<view class="flex1 p-2 bT-f5 p-fX bottom-0 bg-white">
			<view class="btn80 Mgb"
			@click="submit">订单支付</view>
		</view>
		
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				mainData:{},
				chooseSku:{},
				orderList:[],
				isAgree:false
			}
		},
		
		onLoad(options) {
			const self = this;
			self.id = options.id;
			
			self.$Utils.loadAll(['getMainData'], self);
		},
		
		onShow() {
			const self = this;
			self.getUserInfoData();
			self.orderList = [];
			if(uni.getStorageSync('chooseSku')){
				self.chooseSku = uni.getStorageSync('chooseSku')
			}
		},
		
		methods: {
			
			select(){
				const self = this;
				self.isAgree = !self.isAgree
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
				if(!self.isAgree){
					uni.showModal({
						title:'提示',
						content:'请选择同意服务协议',
						showCancel:false,
						confirmText:'我知道了'
					})
					return
				};
				if(JSON.stringify(self.chooseSku)!='{}'){
					self.orderList.push(
						{sku_id:self.chooseSku.id,count:1,product:self.mainData,sku:self.chooseSku},
					);
					uni.setStorageSync('payPro', self.orderList);
					self.Router.navigateTo({route:{path:'/pages/onlineOrder/onlineOrder'}})
				}else{
					
					self.Router.navigateTo({route:{path:'/pages/setMealDetail/setMealDetail?behavior='+behavior}})
				}
			},
			
			choose(index,type){
				const self = this;
				self.chooseSku = self.mainData[type][index]
			},
			
			
			
			getMainData() {
				var self = this;
				var postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
					id:self.id
				};
				postData.getAfter = {
					sku:{
						tableName:'Sku',
						middleKey:'product_no',
						key:'product_no',
						searchItem:{
							status:1
						},
						condition:'='
					}
				};
				var callback = function(res) {
					if (res.info.data.length > 0) {
						self.mainData  = res.info.data[0]
						const regex = new RegExp('<img', 'gi');
						self.mainData.content = self.mainData.content.replace(regex, `<img style="max-width: 100%;height:auto"`);
						if(self.mainData.carcharter.length>0){
							self.chooseSku = self.mainData.carcharter[0]
						}else if(self.mainData.carpool.length>0){
							self.chooseSku = self.mainData.carpool[0]
						};
						uni.setStorageSync('productNo', self.mainData.product_no);
						uni.setStorageSync('carcharterData',self.mainData.carcharter)
						uni.setStorageSync('carpoolData',self.mainData.carpool)
					};
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.productGet(postData, callback);
			},
			
		}
	}
</script>

<style scoped>
.swiper-box,.swiper-item,swiper image{height: 500rpx;width: 100%;}
.xcIcon{width: 41rpx;height: 36rpx;}
.ggIcon{width: 38rpx;height: 36rpx;}

.online image{display: inline-block;}
.onlineCon{width: 600rpx;}
.time::before{content: '';border-radius: 50%;border: 1px solid #888;width: 12rpx;height: 12rpx;position: absolute;top: 6rpx;left: 0;}
.online::before{content: '';border-left: 1px solid #888;width: 1rpx;height: 65%;position: absolute;left: 6rpx;bottom: 10rpx;}

.tc{width: 220rpx;height: 128rpx;margin-right: 25rpx;background-color: #f5f5f5;}
.tc:nth-child(3){margin-right: 0;}
.tcSign{position: absolute;width: 90rpx;line-height: 40rpx;text-align: center;border-radius: 10rpx 0 10rpx 0;border: 1px solid #f1f1f1;background-color: #fff;top: -20rpx;left: 50%;margin-left: -45rpx;}
.on{background-color: #13C3F6;border: none;}
.on .tcSign{border: 1px solid #13C3F6;color: #13C3F6;}
.on .price1{color: #fff;}

.fw{padding: 30rpx 0 30rpx;text-align: center;}
.btn80{color: #fff;border-radius: 10rpx;width: 100%;}
</style>
