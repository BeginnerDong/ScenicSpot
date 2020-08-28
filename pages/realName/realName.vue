<template>
	<view class="px-2">
		
		<view class="py-4 bB-f5 flex1 line-h">
			<view>姓名</view>
			<input type="text" value="" placeholder="填输入" placeholder-style="color:#999;" />
		</view>
		<view class="py-4 bB-f5 flex1 line-h">
			<view>电话</view>
			<input type="text" value="" placeholder="填输入" placeholder-style="color:#999;" />
		</view>
		<view class="py-4 bB-f5 flex1 line-h">
			<view>身份证号</view>
			<input type="text" value="" placeholder="填输入" placeholder-style="color:#999;" />
		</view>
		
		<view class="btn80 Mgb colorf">确定</view>
		
		
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				
			}
		},
		methods: {
			
		}
	}
</script>

<style scoped>
input{flex: 1;text-align: right;line-height: 1;font-size: 22rpx;height: 22rpx;}
.btn80{margin-top: 200rpx;}
</style>