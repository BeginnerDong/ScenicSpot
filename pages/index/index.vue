<template>
	<view>
		<!-- 定位 -->
		<view class="flex1 font-24 p-2">
			<view class="flex">
				<image src="../../static/images/home-icon.png" class="dw-icon mr-1"></image>
				<view>{{district}}</view>
			</view>
			<view class="flex" @click="Router.navigateTo({route:{path:'/pages/store/store?id='+shopData[0].id}})">
				<view>{{shopData[0]&&shopData[0].name?shopData[0].name:''}}</view>
				<image src="../../static/images/home-icon1.png" class="sj-icon ml-1"></image>
			</view>
		</view>
		<!-- banner -->
		<swiper class="swiper-box mx-2 py-1" indicator-dots="indicatorDots" :autoplay="autoplay"  interval="5000"
		 indicator-active-color="#fff">
			<block>
				<swiper-item class="swiper-item" v-for="(item,index) in sliderData" :key="index">
					<image v-if="item.mainImg&&item.mainImg[0]&&item.mainImg[0].type=='image'" :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''"
					 mode="widthFix" />
					<video id="myVideo" loop  show-play-btn controls @click="ZhanTing"  objectFit="cover" v-if="item.mainImg&&item.mainImg[0]&&item.mainImg[0].type=='vedio'"
					 :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''"></video>
				</swiper-item>
			</block>
		</swiper>

		<!-- 优惠券 -->
		<view class="p-3 p-r colorf" v-if="couponData.value">
			<image src="../../static/images/home-img.png" mode="widthFix"></image>
			<view class="p-aXY mx-3 d-flex j-end a-center">
				<view class="font-20"><text class="font-80 font-w">{{couponData.value?couponData.value:''}}</text>元</view>
				<viwe class="text-right line-h">
					<view class="font-24 pb-2">满{{couponData.condition?couponData.condition:''}}可用</view>
					<view class="font-20">({{couponData.behavior==1?'整车':'拼车'}}专享)</view>
				</viwe>
				<view class="btn50 shadowM mx-4" @click="couponAdd()">立即领取</view>
			</view>
		</view>

		<!-- 线路推荐 -->
		<view class="font-30 font-w px-2 pt-2">线路推荐</view>
		<view class="mx-2 pt-4 flex1" v-for="(item,index) in shopData" :key="index" 
		v-if="item.product&&item.product[0]"
		:data-id = "item.product[0].id"
		@click="Router.navigateTo({route:{path:'/pages/onlineDetail/onlineDetail?id='+$event.currentTarget.dataset.id}})">
			<image :src="item.product&&item.product[0]&&item.product[0].mainImg&&item.product[0].mainImg[0]?item.product[0].mainImg[0].url:''" class="wh240 radius10"></image>
			<view class="h240 flex5 pl-2 flex-1">
				<view class="font-30 avoidOverflow2">{{item.product&&item.product[0]?item.product[0].title:''}}</view>
				<view class="font-24 color8 flex-1 pt-2">{{item.product&&item.product[0]?item.product[0].description:''}}</view>
				<view>
					<text class="price1 font-34 font-w">{{item.product&&item.product[0]&&item.product[0].skuArr&&item.product[0].skuArr[0]?item.product[0].skuArr[0].price:''}}</text>
					<text class="font-20 color9 pl-2">已售 {{item.product&&item.product[0]?item.product[0].sale_count:'0'}}</text>
				</view>
			</view>
		</view>

		<!-- 优惠券领取 -->
		<view class="bg-mask" v-show="is_show">
			<view class="hb p-r">
				<image src="../../static/images/home-icon3.png" mode="widthFix"></image>
				<view class="colorf font-50 text-center p-aXY mm">
					<view>满{{couponData.condition}}减{{couponData.value}}</view>
					<view class="font-34 pt-4">领取成功</view>
				</view>
			</view>
			<image src="../../static/images/home-icon4.png" class="wh60 m-a" @click="isShow"></image>
		</view>


		<view class="bg-mask z1000 line-h" v-show="is_show1">
			<view class="p-aX bottom-0 radius20-T bg-white font-30 pt-3 flex5">
				<view class="text-center" style="font-weight: 700;font-size:18px">提示</view>
				<view class="text-center" style="padding:50rpx 0;">请同意授权使用小程序</view>
				<view class="flex0 bg-white py-2 bT-f5">
					<view class="grayBtn linearBtn" style="margin: 0 20rpx;" @click="closeShow1">暂不授权</view>
					<button class="grayBtn linearBtn" style="margin: 0 20rpx;" open-type="getUserInfo" @getuserinfo="Utils.stopMultiClick(submit)">确定</button>
				</view>
			</view>
		</view>
		
		<view class="bg-mask z1000 line-h" v-show="is_show2">
			<view class="p-aX bottom-0 radius20-T bg-white font-30 pt-3 flex5">
				<view class="text-center" style="font-weight: 700;font-size:18px">提示</view>
				<view class="text-center" style="padding:50rpx 0;">查看服务协议</view>
				<button class="flex0 bg-white py-2 bT-f5" @click="Router.navigateTo({route:{path:'/pages/service/service'}})">
					<view class="grayBtn linearBtn">确定</view>
				</button>
			</view>
		</view>
		
		<view style="height: 130rpx;"></view>
		<!-- footer -->
		<view class="footer">
			<view class="item on">
				<image src="../../static/images/nabar1-a.png" mode=""></image>
				<view>首页</view>
			</view>
			<view class="item" @click="Router.redirectTo({route:{path:'/pages/integralShop/integralShop'}})">
				<image src="../../static/images/nabar2.png" mode=""></image>
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
				Router: this.$Router,
				is_show: false,
				sliderData: [],
				autoplay: true,
				couponData:{},
				district:'',
				shopData:[],
				mainData:[],
				is_show1:false,
				Utils:this.$Utils,
				is_show2:false
			}
		},
		
		onLoad() {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.$Utils.loadAll(['getSliderData','getCouponData','getLocation'], self);
			
		},
		
		onShow() {
			const self = this;
			self.getUserData();
			/* if(uni.getStorageSync('shopData')){
				self.district = uni.getStorageSync('shopData').region;
				
				self.shopData = uni.getStorageSync('shopData');
				self.getMainData(true)
			} */
		},
		onReachBottom() {
			console.log('onReachBottom')
			const self = this;
			if (!self.isLoadAll && uni.getStorageSync('loadAllArray')) {
				self.paginate.currentPage++;
				self.getShopData()
			};
		},
		methods: {
			
			closeShow1(){
				const self = this;
				self.is_show1 = false
			},
			
			submit() {
				const self = this;
				
				const callback = (user, res) => {
					console.log(res)
					self.getUserData(user);
				};
				self.$Utils.getAuthSetting(callback);
			},
			
			getUserData(user) {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.noLoading = true;
				if(user){
					postData.refreshToken = true;
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.userData = res.info.data[0];
						if(self.userData.headImgUrl == ''){
							self.is_show1 = true
						}else{
							self.is_show1 = false
						}
						if(self.userData.info.is_read==0){
							self.is_show2 = true
						}else{
							self.is_show2 = false
						}
					};
					uni.setStorageSync('canClick', true);	
				};
				self.$apis.userGet(postData, callback);
			},
			
			getLocation() {
				const self = this;
				const callback = (res) => {
					if (res) {
						if (res.authSetting) {
							self.$Utils.showToast('请至小程序设置中打开位置权限，以便我们提供更好的服务')
							return
						};
						self.district = res.ad_info.district;
						self.lat = res.location.lat;
						self.lng = res.location.lng
						//self.city = res.address_component.city;
						//self.province = res.address_component.province.substring(0, res.address_component.province.length - 1);
						self.getShopData()
					}
					console.log('res', res)
					self.$Utils.finishFunc('getLocation');
				};
				self.$Utils.getLocation('reverseGeocoder', callback);
			},
			
			getShopData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					thirdapp_id: 2,
					user_type:1,
					behavior:1
				};
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.order = {
					distance:'asc',
					longitude:self.lng,
					latitude:self.lat,
				};
				postData.getAfter = {
					product:{
						tableName:'Product',
						middleKey:'user_no',
						key:'shop_no',
						searchItem:{
							status:1
						},
						condition:'='
					}
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.shopData.push.apply(self.shopData, res.info.data);
					}
				};
				self.$apis.userInfoGet(postData, callback);
			},
			
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
					//shop_no:self.shopData.user_no,
					type:1
				};
				/* postData.order = {
					distance:'asc',
					longitude:self.lng,
					latitude:self.lat,
				}; */
				postData.getAfter = {
					sku:{
						tableName:'Sku',
						middleKey:'product_no',
						key:'product_no',
						searchItem:{
							status:1
						},
						condition:'=',
						order:{
							listorder:'desc'
						}
					}
				};
				var callback = function(res) {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
					};
				};
				self.$apis.productGet(postData, callback);
			},
			
			getSliderData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
					menu_id: 2
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.sliderData = res.info.data
					}
					self.$Utils.finishFunc('getSliderData');
				};
				self.$apis.articleGet(postData, callback);
			},
			
			ZhanTing() {
				const self = this;
				self.autoplay = !self.autoplay
			},

		
			
			getCouponData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.couponData = res.info.data[0]
					}
					self.$Utils.finishFunc('getCouponData');
				};
				self.$apis.couponGet(postData, callback);
			},
			
			couponAdd() {
				const self = this;
				var data = {
					behavior:self.couponData.behavior
				};
				self.orderList = [{
					coupon_id: self.couponData.id,
					count: 1,
					type: self.couponData.type,
					data:data
				}];
				const postData = {
					tokenFuncName: 'getProjectToken',
					data:data
				};
				postData.couponList = self.$Utils.cloneForm(self.orderList);
				postData.pay = {
					score: {
						price: 0
					}
				};
				console.log('postData', postData)
				const callback = (res) => {
					if (res && res.solely_code == 100000) {
						self.isShow()
					} else {
						self.$Utils.showToast(res.msg, 'none')
					}
				};
				self.$apis.couponAdd(postData, callback);
			},

			isShow() {
				const self = this;
				self.is_show = !self.is_show;
			}

		}
	};
</script>

<style scoped>
	.swiper-box,
	.swiper-item,
	swiper image,
	video {
		width: 710rpx;
		height: 300rpx;
		border-radius: 10rpx;
	}

	.btn50 {
		width: 150rpx;
		background-image: linear-gradient(to right, #FF5F05, #FEC011);
	}

	.hb {
		width: 440rpx;
		height: 554rpx;
		margin: 30% auto 20%;
	}

	.mm {
		margin-top: 300rpx;
	}
	.grayBtn{background-color: #dcdcdc;color: #666;line-height: 80rpx;border-radius: 40rpx;text-align: center;font-size: 30rpx;width: 260rpx;}
	.linearBtn{background-image: linear-gradient(to right,#13C3F6,#1E90FF);color: #fff;}
</style>
