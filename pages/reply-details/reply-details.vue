<template>
	<view>
		<van-panel :title="dataTable.course" :desc="'教师：' + dataTable.teacher" :status="dataTable.date">
			<van-cell title="课程小结" is-link arrow-direction="down" :label="dataTable.desc"></van-cell>
			<view class="button">
				<van-button type="primary" block open-type="contact">联系老师</van-button>
			</view>
		</van-panel>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				dataTable: {}
			};
		},
		onLoad(option){ //option为object类型，会序列化上个页面传递的参数
			this.dataTable = option
			console.log(option)
		}
	}
</script>

<style lang="scss">
	.button {
		width: 95%;
		margin-top: 50rpx;
		margin-left: 2.5%;
	}
</style>
