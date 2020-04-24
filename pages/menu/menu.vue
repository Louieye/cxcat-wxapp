<template>
	<view class="main">
		<navigator url="/pages/class-table/class-table">
			<image src="../../static/icon_menu/kechengbiao.png" mode="" @click="toClassTable"></image>
			<text>
				课程表
			</text>
		</navigator>
		<navigator url="/pages/reply/reply">
			<image src="../../static/icon_menu/liuyan.png" mode=""></image>
			<text>
				课堂反馈
			</text>
		</navigator>
		<navigator url="/pages/selectTest/selectTest">
			<image src="../../static/icon_menu/lianxi.png" mode=""></image>
			<text>
				小测验
			</text>
		</navigator>
		<navigator url="/pages/works/works">
			<image src="../../static/icon_menu/works.png" mode=""></image>
			<text>
				作品欣赏
			</text>
		</navigator>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				
			};
		},
		methods:{

		}
	}
</script>

<style lang="scss">
	.main {
		display: flex;
		justify-content: space-around;
		navigator {
			height: 100rpx;
			width: 100rpx;
			padding: 10rpx;
			border-radius: 10rpx;
			text {
				font-size: 25rpx;
				width: 100%;
				display: inline-block;
				text-align: center;
			}
			image {
				width: 100%;
				height: 100%;
			}
		}
	}
</style>
