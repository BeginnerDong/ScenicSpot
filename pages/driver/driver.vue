<template>
	<view class="pb-5">
		
		<view class="p-r p-2">
			<view class="Mgb p-aX top-0" style="height: 240rpx;"></view>
			<view class="p-r font-30 colorf line-h flex">
				<image src="../../static/images/tehdriver-img.png" class="wh110 my-2"></image>
				<view class="pl-2">
					<view class="pb-3 font-w">六六六</view>
					<view class="sign">司机</view>
				</view>
				<view class="font-26 p-a top-0 right-0">退出</view>
			</view>
		</view>
		
		<view class="bg-white radius10 flex1 font-24 line-h p-r mx-2 mb-3">
			<view class="w33 flex4 py-3 p-r">
				<view class="font-40 font-w pb-2">12</view>
				<view class="color6">订单总数</view>
			</view>
			<view class="w33 flex4 py-3 p-r"
			@click="Router.navigateTo({route:{path:'/pages/driverProfit/driverProfit'}})">
				<view class="font-40 font-w pb-2">256.22</view>
				<view class="color6">收益</view>
			</view>
			<view class="w33 flex4 py-3 p-r">
				<view class="font-40 font-w pb-2">5</view>
				<view class="color6">今日订单数</view>
			</view>
		</view>
		
		<view class="bg-white pt-3 shadow">
			<view class="mx-2 radius10 borderM overflow-h h70 text-center flex">
				<view class="w-50" :class="navCurr==0?'Mgb colorf':''" @click="change('nav',0)">包车订单</view>
				<view class="w-50" :class="navCurr==1?'Mgb colorf':''" @click="change('nav',1)">拼车订单</view>
			</view>
			<view class="list">
				<view class="li" :class="liCurr==0?'on':''" @click="change('li',0)">全部</view>
				<view class="li" :class="liCurr==1?'on':''" @click="change('li',1)">待发车</view>
				<view class="li" :class="liCurr==2?'on':''" @click="change('li',2)">已完成</view>
			</view>
		</view>
		
		<view class="bg-white radius10 mx-2 px-2 mt-3" v-show="navCurr==0"
		@click="Router.navigateTo({route:{path:'/pages/onlineOrder-detail/onlineOrder-detail'}})">
			<view class="font-24 color8 flex1 py-2">
				<view>交易时间：2019.12.30</view>
				<view class="colorR">待发车</view>
			</view>
			<view class="flex1 pb-2 bB-df5">
				<image src="../../static/images/thaorder-img1.png" class="wh180 radius10"></image>
				<view class="h180 flex5 pl-2 flex-1">
					<view class="avoidOverflow2 font-w">西安秦始皇兵马俑博物馆+华清池+骊山+一日游【冰蛋语音讲解/长恨歌】</view>
					<view class="flex1">
						<view class="price1 font-34 font-w">2662</view>
						<view>4人</view>
					</view>
				</view>
			</view>
			<view class="flex1 py-3 bB-df5">
				<view>套餐：</view>
				<view>包车套餐（A套餐） x2</view>
			</view>
			<view class="flex1 py-2 bB-df5">
				<view>二维码：</view>
				<image src="../../static/images/tehdriver-img1.png" class="wh80"></image>
			</view>
			<view class="py-2 text-right">
				<view class="btn text-center font-26 b-e1 radius10">确认发车</view>
			</view>
		</view>
		
		
		
		<view class="bg-white radius10 mx-2 px-2 mt-3" v-show="navCurr==1"
		@click="Router.navigateTo({route:{path:'/pages/onlineOrder-detail/onlineOrder-detail'}})">
			<view v-for="v in 3" :key="v">
				<view class="font-24 color8 flex1 py-2">
					<view>交易时间：2019.12.30</view>
					<view class="colorR">已完成</view>
				</view>
				<view class="flex1 pb-2 bB-df5">
					<image src="../../static/images/home-img1.png" class="wh180 radius10"></image>
					<view class="h180 flex5 pl-2 flex-1">
						<view class="avoidOverflow2 font-w">西安秦始皇兵马俑博物馆+华清池+骊山+一日游【冰蛋语音讲解/长恨歌】</view>
						<view class="flex1">
							<view class="price1 font-34 font-w">2662</view>
							<view>4人</view>
						</view>
					</view>
				</view>
				<view class="flex1 py-3 bB-df5">
					<view>套餐：</view>
					<view>拼车套餐（A套餐） x2</view>
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
				liCurr:0,
				navCurr:0
			}
		},
		methods: {
			
			change(type,i){
				const self = this;
				if(type == 'nav'){
					self.navCurr =i;
				}else{
					self.liCurr = i;
				}
			}
			
		}
	}
</script>

<style>
page{background-color: #f5f5f5;}
</style>
<style scoped>

.w33,.li{width: 33.33%;}
.w33::before{content: '';height: 40rpx;border-left: 1px solid #f5f5f5;position: absolute;right: 0;top: 50%;margin-top: -20rpx;}

.h70{line-height: 70rpx;}

.btn{width: 150rpx;line-height: 60rpx;display: inline-block;}
</style>
