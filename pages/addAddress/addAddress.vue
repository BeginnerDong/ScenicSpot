<template>
	<view class="px-2">
		
		<view class="py-4 bB-f5 flex1 line-h">
			<view>姓名</view>
			<input type="text" v-model="submitData.name" placeholder="填写姓名" placeholder-style="color:#999;" />
		</view>
		<view class="py-4 bB-f5 flex1 line-h">
			<view>手机号</view>
			<input type="text" v-model="submitData.phone" placeholder="填写手机号" placeholder-style="color:#999;" />
		</view>
		<view class="py-4 bB-f5 flex1 line-h">
			<view>所在地区</view>
			<picker mode="region" class="flex-1 text-right"  @change="cityChange">
				<view class="color9" :style="submitData.city!=''?'color:#222':''">{{submitData.city==''?'请选择所在地区':submitData.city}}</view>
			</picker>
			<image src="../../static/images/icon.png" class="R-icon ml-1"></image>
		</view>
		<view class="py-4 bB-f5 flex1 line-h">
			<view>详细地址</view>
			<input type="text"  v-model="submitData.detail" placeholder="请填写" placeholder-style="color:#999;" />
		</view>
		<view class="py-4 bB-f5 flex1 line-h">
			<view>设为默认地址</view>
			<image @click="changeDefault" 
			:src="submitData.isdefault==1?'../../static/images/address-icon5.png':'../../static/images/address-icon6.png'"  
			 class="mr-icon"></image>
			<!-- <image src="../../static/images/address-icon6.png" class="mr-icon"></image> -->
		</view>
		
		<view class="btn80 Mgb colorf mt-4"  @click="Utils.stopMultiClick(submit)">确定</view>
		
		
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				submitData: {
					name: '',
					city:'',
					detail: '',
					phone:'',
					isdefault:0,
				},
				Utils:this.$Utils,
				id:0,
			}
		},
		
		onLoad(options) {
			const self = this;
			uni.setStorageSync('canClick', true);
			if(options.id){
				self.id = options.id;
				self.$Utils.loadAll(['getMainData'], self);
				uni.setNavigationBarTitle({
					title:'编辑地址'
				})
			}
		},
		
		methods: {
			
			
			
			changeDefault(){
				const self = this;
				if(self.submitData.isdefault==0){
					self.submitData.isdefault = 1
				}else{
					self.submitData.isdefault = 0
				}
			},
			
			cityChange(e){
				const self = this;
				self.submitData.city = e.detail.value[0]+e.detail.value[1]+e.detail.value[2];
				//self.check();
			},
			
			
			
			getMainData(id) {
				const self = this;
				const postData = {};
				postData.searchItem = {};
				postData.searchItem.id = self.id;
				postData.tokenFuncName = 'getProjectToken';
			
				const callback = (res) => {
					console.log(res);
					self.submitData.name = res.info.data[0].name;
					self.submitData.city = res.info.data[0].city;
					self.submitData.detail = res.info.data[0].detail;
					self.submitData.phone = res.info.data[0].phone;
					self.submitData.isdefault = res.info.data[0].isdefault;
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.addressGet(postData, callback);
			},
			
			
			
			addressUpdate() {
				const self = this;
				const postData = {};
			
				postData.tokenFuncName = 'getProjectToken';
			
				postData.searchItem = {};
				postData.searchItem.id = self.id;
				postData.data = {};
				postData.data = self.$Utils.cloneForm(self.submitData);
			
				const callback = (data) => {
					uni.setStorageSync('canClick', true);
					if (data && data.solely_code == 100000) {
						self.$Utils.showToast('编辑成功','success');
						setTimeout(function() {
							uni.navigateBack({
								delta:1
							})
						}, 1000);
					} else {
						self.$Utils.showToast(data.msg,'error')
					};
					
				};
				self.$apis.addressUpdate(postData, callback);
			},
			
			
			addressAdd() {
				const self = this;
				const postData = {};
			
				postData.tokenFuncName = 'getProjectToken';
			
				postData.data = {};
				postData.data = self.$Utils.cloneForm(self.submitData);
				const callback = (data) => {
					uni.setStorageSync('canClick', true);
					if (data && data.solely_code == 100000) {
						self.$Utils.showToast('添加成功','success');
						setTimeout(function() {
							uni.navigateBack({
								delta:1
							})
						}, 1000);
					} else {
						self.$Utils.showToast(data.msg,'success')
					}
					
				};
				self.$apis.addressAdd(postData, callback);
			},
			
			
			submit() {
				const self = this;
				console.log(34)
				
				console.log(12)
				uni.setStorageSync('canClick', false);
				var newObject = self.$Utils.cloneForm(self.submitData);
				delete newObject.isdefault;
				const pass = self.$Utils.checkComplete(newObject);
				
				console.log('self.data.sForm', self.submitData)
				console.log('pass', pass)
				if (pass) {
					if (self.submitData.phone.trim().length != 11 || !/^1[3|4|5|6|7|8|9]\d{9}$/.test(self.submitData.phone)) {
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast('请输入正确的手机号', 'none', 1000)
						return;
					}
					if (self.id) {
			
						self.addressUpdate();
					} else {
			
						self.addressAdd();
					}
			
			
				} else {
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('请补全信息','success');
				};
			},
			
		}
	}
</script>

<style scoped>
input{flex: 1;text-align: right;line-height: 1;height: 22rpx;}
.mr-icon{width: 67rpx;height: 34rpx;}
</style>
