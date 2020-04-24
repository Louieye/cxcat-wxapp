<template>
	<view class="main">
		<icon type="success" size="70"/>
		<view>操作成功，3秒后自动返回首页</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				
			};
		},
		onLoad(){
			setTimeout(()=>{
				uni.switchTab({
					url:"/pages/home/home"
				})
			},3000)
		},
		methods:{
			
		}
	}
</script>

<style lang="scss">
	.main {
		display: flex;
		flex-direction: column;
		align-items: center;
		view {
			margin-top: 80rpx;
		}
	}
</style>
