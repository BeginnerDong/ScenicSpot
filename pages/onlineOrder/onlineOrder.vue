<template>
	<view>
		
		<view class="pt-4 py-3 line-h">
			<image src="../../static/images/thaorder-icon.png" mode="widthFix"></image>
			<view class="py-3 px-2">{{shopData.name?shopData.name:''}} {{shopData.phone?shopData.phone:''}}</view>
			<view class="px-2">{{shopData.address?shopData.address:''}}</view>
		</view>
		<view class="f5Bj-H20"></view>
		
		<view class="px-2 py-3 flex1">
			<image :src="mainData.product&&mainData.product.mainImg&&mainData.product.mainImg[0]?mainData.product.mainImg[0].url:''" class="wh180 radius10"></image>
			<view class="h180 flex5 pl-2 flex-1 font-w">
				<view class="avoidOverflow2">{{mainData.product&&mainData.product.title?mainData.product.title:''}}</view>
				<view class="price1 font-34">{{mainData.sku&&mainData.sku.price?mainData.sku.price:''}}</view>
			</view>
		</view>
		<view class="f5Bj-H20"></view>
		
		<view>
			<view class="px-2">
				<view class="py-4 bB-f5 flex1">
					<view>购买类型</view>
					<view>{{mainData.sku&&mainData.sku.behavior==1?'整车':'拼车'}}</view>
				</view>
				<!-- <view class="py-4 bB-f5 flex1">
					<view>购买人数</view>
					<view class="flex color9 font-24">
						<picker mode="selector" :range="num"   @change="numChange">
							<view class="color9" :style="submitData.city!=''?'color:#222':''">{{standard>0?standard+'人':'请选择'}}</view>
						</picker>
						<image src="../../static/images/icon.png" class="R-icon ml-1"></image>
					</view>
				</view> -->
				<view class="py-4 bB-f5 flex1">
					<view>套餐内容</view>
					<view>{{mainData.sku&&mainData.sku.title?mainData.sku.title:''}} x{{mainData.count}}</view>
				</view>
				<view class="py-4 bB-f5 flex1">
					<view>座位数量</view>
					<view class="count">
						<view class="countIcon" @click="counter('-')"><image src="../../static/images/detailsonline-icon5.png" class="countIcon1"></image></view>
						<view class="countNum">{{mainData.count}}</view>
						<view class="countIcon" @click="counter('+')"><image src="../../static/images/detailsonline-icon6.png" class="countIcon2"></image></view>
					</view>
				</view>
				<view class="py-3 bB-f5 flex1">
					<view>优惠券</view>
					<view class="d-flex j-end a-center color6 font-26" v-if="couponData.length==0">
						暂无优惠券使用
						
					</view>
					<view class="d-flex j-end a-center color6 font-26" v-if="couponData.length>0&&chooseCoupon.length==0" @click="couponShow">
						{{couponData.length}}张优惠券
						
					</view>
					<view class="d-flex j-end a-center color6 font-26" v-if="couponData.length>0&&chooseCoupon.length>0" @click="couponShow">
						优惠券抵扣-{{pay.coupon[0].price}}
						
					</view>
				</view>
			</view>
			<view class="f5Bj-H20"></view>
		</view>
		
		
		<view style="height: 140rpx;"></view>
		<view class="flex1  p-fX bottom-0 bg-white shadowM">
			<view class="pl-3 flex">合计
				<text class="font-34 font-w price1 pl-1" >{{realMoney}}</text>
			</view>
			
			<view class="btn80 Mgb"
			@click="Utils.stopMultiClick(submit)">立即支付</view>
		</view>
		
		<view class="black-bj" v-show="is_show1"></view>
		<view class="couponShow whiteBj radius10" v-show="is_couponShow">
			<view class="d-flex j-sb px-3 py-3 border-bottom">
				<view class="font-26 color9" @click="couponShow">取消</view>
				<view class="">优惠券</view>
				<view class="font-26 main-text-color"  @click="useCoupon">确定</view>
			</view>
			
			
			<view class="pt-1 font-26">
				<view class="item d-flex j-sb a-center px-3 mt-3" v-for="(item,index) in couponData" :key="index" 
				@click="couponChange(index)">
					<view>满{{item.condition}}减{{item.value}}元</view>
					<view class="seltIcon"><image :src="couponCurr==index?'../../static/images/address-icon3.png':'../../static/images/address-icon2.png'" mode=""></image></view>
				</view>
			</view>
		</view>
		
		<!-- 人数弹窗 -->
		<view class="bg-mask" v-show="is_show">
			<view class="maskCon radius10 overflow-h ">
				<view class="p-3 flex1 bB-f5" v-for="v in 8" :key="v">
					<view>{{v}}人</view>
					<image src="../../static/images/thaorder-icon1.png" class="yes-icon"></image>
				</view>
			</view>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				is_show:false,
				
				totalPrice:0,
				mainData:{},
				shopData:{},
				standard:0,
				num:['1','2','3','4','5','6','7','8'],
				Utils:this.$Utils,
				couponData:[],
				pay:{
					coupon:[]
				},
			
				chooseCoupon:[],
				is_couponShow:false,
				is_show1:false,
				couponCurr:-1,
				realMoney:0
			}
		},
		onLoad() {
			const self = this;
			self.mainData = uni.getStorageSync('payPro')[0];
			self.countTotalPrice();
			self.$Utils.loadAll(['getShopData','getUserCouponData'], self);
		},
		
		methods: {
			
			
			useCoupon() {
				const self = this;	
				self.couponTotalPrice = self.$Utils.addItemInArray(self.pay.coupon, 'price');
			/* 	console.log(index)
				console.log(self.couponData);
				console.log(self.couponData[0]); */
				/* for (var i = 0; i < self.mainData.length; i++) {
					self.totalPrice += parseFloat(parseFloat(self.mainData[i].product.sku[self.mainData[i].skuIndex].price)*parseFloat(self.mainData[i].count)).toFixed(2)
				}; */
				console.log('self.totalPrice', self.totalPrice)
				if(self.couponData[self.couponCurr]&&self.couponData[self.couponCurr].id){
					var id = self.couponData[self.couponCurr].id;
				}else{
					self.pay.coupon = []
					self.chooseCoupon = [];
					self.is_show1 = !self.is_show1;
					self.is_couponShow = !self.is_couponShow;
					self.countTotalPrice();
					console.log('self.pay',self.pay)
					return
				}
				
				var findCoupon = self.$Utils.findItemInArray(self.couponData, 'id', id);
				var findItem = self.$Utils.findItemInArray(self.pay.coupon, 'id', id);
				console.log('findCoupon', findCoupon)
				self.showCoupon = false;
				
				if (findCoupon) {
					findCoupon = findCoupon[1];
					var findSameCoupon =  self.$Utils.findItemsInArray(self.pay.coupon, 'product_id', id);
				} else {
					self.$Utils.showToast('优惠券错误', 'none');
					return;
				};
				if (findItem) {
					self.is_show1 = !self.is_show1;
					self.is_couponShow = !self.is_couponShow;
					self.countTotalPrice();
					console.log('self.pay',self.pay)
					return
				} else {
					console.log('self.totalPrice', self.totalPrice)
					console.log('self.totalPrice', self.couponTotalPrice)
					console.log('self.totalPrice', findCoupon.condition)
					if ((self.totalPrice - self.couponTotalPrice) < findCoupon.condition) {
						self.$Utils.showToast('未达满减标准', 'none');				
						return;
					};			
					self.pay = {
						coupon:[]
					};
					console.log('findSameCoupon.length', findSameCoupon.length)
					if (self.pay.coupon.length >= 1) {
						self.$Utils.showToast('叠加使用超限', 'none');
					
						return;
					};
					
					if (findCoupon.type == 1) {
						var couponPrice = findCoupon.value;
						console.log('findCoupon.discount', findCoupon.discount)
					} else if (findCoupon.type == 2) {
						
						var couponPrice = parseFloat(self.totalPrice).toFixed(2) - parseFloat(findCoupon.discount / 100 * self.totalPrice)
							.toFixed(2);
					};
					/* if (parseFloat(couponPrice) + parseFloat(self.couponTotalPrice) > parseFloat(self.totalPrice)) {
						couponPrice = parseFloat(self.totalPrice).toFixed(2) - parseFloat(self.couponTotalPrice).toFixed(2);
					}; */
					self.pay.coupon.push({
						id: id,
						price: couponPrice.toFixed(2),
					});
					self.chooseCoupon.push({
						id: id,
						price: couponPrice,
					});
				};
				self.is_show1 = !self.is_show1;
				self.is_couponShow = !self.is_couponShow;
				self.countTotalPrice();
			},
			
			couponChange(index){
				const self = this;
				if(self.couponCurr!=index){
					self.couponCurr = index;
				}else{
					self.couponCurr = -1
				}
			},
			
			couponShow(){
				const self = this;
				self.is_show1 = !self.is_show1;
				self.is_couponShow = !self.is_couponShow;
			},
			
			getUserCouponData() {
				const self = this;
				var now = Date.parse(new Date());
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					use_step: 1,
					behavior:self.mainData.sku.behavior,
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.couponData = res.info.data
					}
					self.$Utils.finishFunc('getUserCouponData');
				};
				self.$apis.userCouponGet(postData, callback);
			},
			
			submit(){
				const self = this;
				uni.setStorageSync('canClick', false);
				/* if(self.standard==0){
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('请选择备注人数','none')
					return
				}; */
				var orderList = []
				orderList.push({sku_id:self.mainData.sku_id,count:self.mainData.count})
				self.addOrder(orderList)
			},
			
			addOrder(orderList) {
				const self = this;	
				const postData = {}; 
				postData.orderList = self.$Utils.cloneForm(orderList);
				postData.type=1;
				postData.data = {
					type:1,
					standard:self.standard,
					shop_no:self.mainData.product.shop_no,
					region:self.shopData.region,
					discount:self.mainData.sku.behavior,
					//pay:self.pay
					price:self.totalPrice
				};
				postData.tokenFuncName = 'getProjectToken';
				const callback = (res) => {
					uni.setStorageSync('canClick', true);
					if (res && res.solely_code == 100000) {
						self.orderId = res.info.id;
						self.goPay()
					} else {		
						uni.showToast({
							title: res.msg,
							duration: 2000
						});
					};		
				};
				self.$apis.addOrder(postData, callback);
			},
			
			goPay() {
				const self = this;	
				const postData = self.$Utils.cloneForm(self.pay);	
				postData.tokenFuncName = 'getProjectToken',
				postData.searchItem = {
					id: self.orderId
				};
				postData.payAfter = [{
					tableName: 'FlowLog',
					FuncName: 'add',
					data: {
						type: 3,
						count: parseFloat(self.mainData.sku.score),
						trade_info: '消费返积分',
						account: 1,
						thirdapp_id: 2,
						user_no: uni.getStorageSync('user_info').user_no
					},
				}, ];
				const callback = (res) => {
					if (res.solely_code == 100000) {
						uni.setStorageSync('canClick', true);
						if (res.info) {
							const payCallback = (payData) => {
								console.log('payData', payData)
								if (payData == 1) {
									self.$Utils.showToast('支付成功','none')
									setTimeout(function() {
										self.$Router.redirectTo({route:{path:'/pages/paySuccess/paySuccess?id='+self.orderId}})
									}, 1000);
								} else {
									uni.setStorageSync('canClick', true);
									self.$Utils.showToast('支付失败','none')
								};
							};
							self.$Utils.realPay(res.info, payCallback);
						} else {
							self.$Utils.showToast('支付成功','none')
							setTimeout(function() {
								self.$Router.redirectTo({route:{path:'/pages/paySuccess/paySuccess?id='+self.orderId}})
							}, 1000);
						};
					} else {
						uni.setStorageSync('canClick', true);
						uni.showToast({
							title: res.msg,
							duration: 2000
						});
					};
				};
				self.$apis.pay(postData, callback);
			},
			
			numChange(e){
				const self = this;
				self.standard = self.num[e.detail.value]
			},
			
			getShopData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					thirdapp_id: 2,
					user_type:1,
					behavior:1,
					user_no:self.mainData.product.shop_no
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.shopData = res.info.data[0];
					}
					self.$Utils.finishFunc('getShopData');
				};
				self.$apis.userInfoGet(postData, callback);
			},
			
			counter(type) {
				const self = this;
				if (type == '+') {
					self.mainData.count++;
				} else {
					if (self.mainData.count > 1) {
						self.mainData.count--;
					}
				};
				self.countTotalPrice();
			},
			
			countTotalPrice() {
				const self = this;
				self.realMoney = 0;
				self.totalPrice = 0;
				self.totalPrice = self.mainData.sku.price*self.mainData.count
				self.couponTotalPrice = self.$Utils.addItemInArray(self.pay.coupon, 'price');
				self.totalPrice = parseFloat(self.totalPrice).toFixed(2)
				self.realMoney = (parseFloat(self.totalPrice) - parseFloat(self.couponTotalPrice)).toFixed(2)
				if (self.totalPrice > 0) {
					self.pay.wxPay = {
						price: self.realMoney,
					};
				} else {
					  delete self.pay.wxPay;	 
				};
				console.log(self.pay)
			},
		}
	}
</script>

<style scoped>
.btn80{color: #fff;border-radius: 0;width: 280rpx;line-height: 100rpx;}
.maskCon{width: 600rpx;margin-top: 25%;}
.couponShow{width: 100%;position: fixed;left: 0;right: 0;bottom: 0;height:500rpx;z-index: 50;padding-bottom: 80rpx;}
	.seltIcon{width: 36rpx;height: 36rpx;}
	.black-bj,.black-bj2 {background: #000;opacity: 0.5;position: fixed;left: 0;top: 0;right: 0;bottom: 0;z-index: 40; }
</style>
