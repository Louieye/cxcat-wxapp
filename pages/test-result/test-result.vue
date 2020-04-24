<template>
	<view class="main">
		<icon type="success" size="70"/>
		<view class="result">
			你的成绩是:{{result}}分
		</view>
		<view class="footer">
			<van-button type="info" block @click="again">再做一次</van-button>
			<van-button type="primary" block @click=goBack>回到首页</van-button>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
		    return {
			  result: ''
		    };
		},
		onLoad(option) {
			this.result = option.result
		},
		methods:{
			again(){
				uni.redirectTo({
					url:"/pages/selectTest/selectTest"
				})
			},
			goBack(){
				uni.switchTab({
					url:"/pages/home/home"
				})
			}
		}
	}
</script>

<style lang="scss">
	page {
		height: 100%;
	}
	.main {
		height: 100%;
		width: 100%;
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: space-between;
		icon {
			padding-top: 200rpx;
		}
	}
	.result {
		padding-bottom: 200rpx;
	}
	.footer {
		width: 90%;
		
	}
</style>
