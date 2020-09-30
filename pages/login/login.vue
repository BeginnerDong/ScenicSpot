<template>
	<view>
		
		<image src="../../static/images/landing-img.png" mode="widthFix" class="p-s top-0 "></image>
		
		<view class="d-flex a-start px-5 mx-1 mt-5 pt-3">
			<image src="../../static/images/zlanding-icon1.png" class="phone"></image>
			<input type="text" v-model="submitData.login_name" placeholder="输入账号" placeholder-style="color:#999" />
		</view>
		<view class="d-flex a-start px-5 mx-1 mt-5 pt-3">
			<image src="../../static/images/landing-icon.png" class="safe"></image>
			<input type="text" v-model="submitData.password" placeholder="输入密码" placeholder-style="color:#999" />
		</view>
		
		<view class="btn80 Mgb" @click="submit">登录</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				submitData:{
					login_name:'',
					password:''
				},
				Router:this.$Router
			}
		},
		onLoad(options) {
			const self = this;
			uni.hideLoading();
			self.type = options.type
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			submit() {
				const self = this;
				const postData = {
					login_name: self.submitData.login_name,
					password:self.submitData.password
				};
				if (self.$Utils.checkComplete(self.submitData)) {
					const callback = (res) => {
						if (res.solely_code == 100000) {
							self.$Utils.showToast('登陆成功','none')
							if(self.type==1){
								uni.setStorageSync('shop_token', res.token);
								uni.setStorageSync('shop_info', res.info);
								setTimeout(function() {
									self.Router.redirectTo({route:{path:'/pages/entrance/entrance?type=1'}})
								}, 1000);
							}else if(self.type==2){
								uni.setStorageSync('driver_token', res.token);
								uni.setStorageSync('driver_info', res.info);
								setTimeout(function() {
									self.Router.redirectTo({route:{path:'/pages/driver/driver'}})
								}, 1000);
							}else if(self.type==3){
								uni.setStorageSync('area_token', res.token);
								uni.setStorageSync('area_info', res.info);
								setTimeout(function() {
									self.Router.redirectTo({route:{path:'/pages/entrance/entrance?type=3'}})
								}, 1000);
							}else if(self.type==4){
								uni.setStorageSync('agent_token', res.token);
								uni.setStorageSync('agent_info', res.info);
								setTimeout(function() {
									self.Router.redirectTo({route:{path:'/pages/entrance/entrance?type=4'}})
								}, 1000);
							}
						} else {
							self.$Utils.showToast(res.msg,'none')
						}
					}
					if(self.type==1){
						self.$apis.shopLogin(postData, callback);
					}else if(self.type==2){
						self.$apis.driverLogin(postData, callback);
					}else if(self.type==3){
						self.$apis.areaLogin(postData, callback);
					}else if(self.type==4){
						self.$apis.agentLogin(postData, callback);
					}
				} else {
					self.$Utils.showToast('请补全登录信息', 'none')
				};
			},
		}
	}
</script>

<style scoped>
input{font-size: 24rpx;margin-left: 30rpx;flex: 1;border-bottom: 1px solid #f5f5f5;padding: 15rpx 0;}
.phone{width: 29rpx;height: 43rpx;}
.safe{width: 36rpx;height: 40rpx;}
.btn80{color: #fff;border-radius: 10rpx;width: 100%;margin: 100rpx auto;width: 640rpx;}
</style>
