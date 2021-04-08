<template>
	<view>
		
		<view class="p-2">
			<view class="content" style="padding:0" v-html="mainData.content">
			</view>
		</view>
		
		
		<view style="height: 10rpx;"></view>
		<view class="Mgb colorf font-30 line-h py-3 p-fX bottom-0 text-center" @click="userInfoUpdate">确定</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				mainData:{},
				Router:this.$Router
			}
		},
		onLoad() {
			const self = this;
			self.$Utils.loadAll(['getMainData'], self);
		},
		onShow() {
			const self = this;
			self.getUserInfoData()
		},
		methods: {
			
			submit(){
				const self = this;
				
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
			
			userInfoUpdate(){
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.data = {
					status:1
				};
				postData.searchItem = {
					user_no:uni.getStorageSync('user_info').user_no
				};
				const callback = (res) => {
					if (res.solely_code==100000) {
						if(self.userInfoData.idCard!=''){
							uni.navigateBack({
								delta:1
							})
						}else{
							uni.showModal({
								title:'提示',
								content:'是否立即前往实名认证',
								success(res) {
									if(res.confirm){
										self.Router.redirectTo({route:{path:'/pages/realName/realName'}})
									}
								}
							})
						}
					}else{
						self.$Utils.showToast(res.msg, 'none');
					}
				};
				self.$apis.userInfoUpdate(postData, callback);
			},
			
			back(){
				uni.navigateBack({
					delta:1
				})
			},
			
			getMainData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
					menu_id: 1
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData = res.info.data[0]
					}
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.articleGet(postData, callback);
			},
		}
	}
</script>

<style>

</style>
