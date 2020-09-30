<template>
	<view>
		
		<view v-for="(item,index) in shopData" :key="index">
			<view class="flex1 py-4 px-2">
				<view class="line-h">
					<view>{{item.name}}</view>
					<view class="color6 font-26 pt-2">{{item.address}}</view>
				</view>
				<image @click="choose(index)" :src="id==item.id?'../../static/images/address-icon3.png':'../../static/images/address-icon2.png'" class="wh32"></image>
				<!-- <image src="../../static/images/address-icon2.png" class="wh32"></image> -->
			</view>
			<view class="f5Bj-H20"></view>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				shopData:[],
				id:''
			}
		},
		onLoad(options) {
			const self = this;
			self.id = options.id;
			self.getLocation()
		},
		methods: {
			
			choose(index){
				const self = this;
				self.id = self.shopData[index].id;
				uni.setStorageSync('shopData',self.shopData[index]);
				uni.navigateBack({
					delta:1
				})
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
					//self.$Utils.finishFunc('getLocation');
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
				postData.order = {
					distance:'asc',
					longitude:self.lng,
					latitude:self.lat,
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.shopData = res.info.data;
					}
				};
				self.$apis.userInfoGet(postData, callback);
			},
			
		}
	}
</script>

<style>

</style>
