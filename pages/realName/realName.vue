<template>
	<view class="px-2">
		
		<view class="py-4 bB-f5 flex1 line-h">
			<view>姓名</view>
			<input type="text" v-model="submitData.name" placeholder="填输入" placeholder-style="color:#999;" />
		</view>
		<view class="py-4 bB-f5 flex1 line-h">
			<view>电话</view>
			<input type="number" v-model="submitData.phone" maxlength="11" placeholder="填输入" placeholder-style="color:#999;" />
		</view>
		<view class="py-4 bB-f5 flex1 line-h">
			<view>身份证号</view>
			<input type="idcard" v-model="submitData.idCard" placeholder="填输入" placeholder-style="color:#999;" />
		</view>
		
		<view class="btn80 Mgb colorf" v-if="userInfoData.name==''" @click="Utils.stopMultiClick(submit)">确定</view>
		
		
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Utils:this.$Utils,
				submitData:{
					name:'',
					phone:'',
					idCard:''
				},
				userInfoData:{}
			}
		},
		onLoad(options) {
			const self = this;
			self.$Utils.loadAll(['getUserInfoData'], self);
		},
		methods: {
			
			submit() {
				const self = this;
				uni.setStorageSync('canClick', false);
				if(self.submitData.name == ''){
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('请填写姓名', 'none')
					return
				};
				if(self.submitData.phone == ''){
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('请填写电话号', 'none')
					return
				};
				if(self.submitData.idCard == ''){
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('请填写身份证号', 'none')
					return
				};
				if (self.submitData.phone.trim().length != 11 || !/^1[3|4|5|6|7|8|9]\d{9}$/.test(self.submitData.phone)) {
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('请输入正确的手机号', 'none', 1000)
					return;
				};
				if(!/^\d{17}(\d|x)$/i.test(self.submitData.idCard)) {
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('你输入的身份证长度或格式错误', 'none', 1000)
					return;
				};
				self.userInfoUpdate();
				/* const pass = self.$Utils.checkComplete(self.submitData);
				console.log('pass', pass);
				console.log('self.submitData',self.submitData)
				
				if (pass) {	
					self.userInfoUpdate();
				} else {
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('请补全信息', 'none')
				}; */
			},
			
			userInfoUpdate(){
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				var newObject = self.$Utils.cloneForm(self.submitData);
				postData.data = self.$Utils.cloneForm(newObject);
				postData.searchItem = {
					user_no:uni.getStorageSync('user_info').user_no
				};
				postData.saveAfter = [
					{
						tableName: 'UserInfo',
						FuncName: 'update',
						data: {
							is_read:1
						},
						searchItem:{
							user_no:uni.getStorageSync('user_info').user_no
						}
					}
				];	
				const callback = (res) => {
					if (res.solely_code==100000) {
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast('完善成功', 'none');
						setTimeout(function() {
							uni.navigateBack({
								delta:1
							})
						}, 1000);
					}else{
						self.$Utils.showToast(res.msg, 'none');
					}
				};
				self.$apis.userInfoUpdate(postData, callback);
			},
			
			getUserInfoData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.userInfoData = res.info.data[0];
						self.submitData.name = self.userInfoData.name
						self.submitData.phone = self.userInfoData.phone
						self.submitData.idCard = self.userInfoData.idCard
					}
					self.$Utils.finishFunc('getUserInfoData');
				};
				self.$apis.userInfoGet(postData, callback);
			},
			
		}
	}
</script>

<style scoped>
input{flex: 1;text-align: right;line-height: 1;height: 22rpx;}
.btn80{margin-top: 200rpx;}
</style>