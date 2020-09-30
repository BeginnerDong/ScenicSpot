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
		
		<!-- 信息 -->
		<view class="px-2 py-3">
			<view class="font-32">{{mainData.title?mainData.title:''}}</view>
			<view class="flex1">
				<view class="font-20 colorR pt-2">
					<text class="font-30 font-w">{{mainData.score?mainData.score:''}}</text>积分
					<text class="font-30 font-w">+{{mainData.price?mainData.price:''}}</text>元
				</view>
				<view class="font-24 color6">库存 {{mainData.stock?mainData.stock:''}}</view>
			</view>
		</view>
		<view class="f5Bj-H20"></view>
		
		<!-- 详情 -->
		<view class="px-2 py-3">
			<view class="color6 pb-3">商品详情</view>
			<view class="content ql-editor" style="padding:0" v-html="mainData.content">
			</view>
		</view>
		
		
		<view style="height: 140rpx;"></view>
		<view class="flex1 p-2 bT-e1 p-fX bottom-0 bg-white">
			<view class="btn80 Mgb"
			@click="submit">立即兑换</view>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				mainData:{}
			}
		},
		
		onLoad(options) {
			const self = this;
			self.id = options.id;
			self.$Utils.loadAll(['getMainData'], self);
		},
		
		onShow() {
			const self = this;
			self.getUserInfoData()
		},
		
		methods: {
			
			submit(){
				const self = this;
				if(self.userInfoData.idCard!=''){
					self.Router.navigateTo({route:{path:'/pages/integralOrder/integralOrder?id='+self.mainData.id}})
				}else{
					uni.showModal({
						title:'提示',
						content:'您还未实名认证，是否立即前往',
						success(res) {
							if(res.confirm){
								self.Router.navigateTo({route:{path:'/pages/realName/realName'}})
							}
						}
					})
				}
			},
			
			getUserInfoData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.userInfoData = res.info.data[0]
					}
					//self.$Utils.finishFunc('getUserInfoData');
				};
				self.$apis.userInfoGet(postData, callback);
			},
			
			getMainData() {
				var self = this;
				var postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
					id:self.id
				};
				var callback = function(res) {
					if (res.info.data.length > 0) {
						self.mainData  = res.info.data[0]
						const regex = new RegExp('<img', 'gi');
						self.mainData.content = self.mainData.content.replace(regex, `<img style="max-width: 100%;height:auto"`);
					};
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.productGet(postData, callback);
			},
			
		}
	}
</script>

<style scoped>
.swiper-box,.swiper-item,swiper image{height: 750rpx;width: 100%;}
.btn80{color: #fff;border-radius: 10rpx;width: 100%;}
</style>
