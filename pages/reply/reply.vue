<template>
	<view>
		<van-loading v-if="isLoading" color="#1989fa" style="margin-left: 45%;margin-top: 40%;" />
		<template v-for="item in dataTable">
			<van-panel :title="item.data[0].course" :desc="'教师： ' + item.data[0].teacher">
				<view class="cell-box">
					<template v-for="v in item.data">
						<van-cell :title="'第' + v.lesson + '课时 '" :value="v.date" is-link @click="toDetails(v.course, v.teacher, v.date, v.desc)"></van-cell>
					</template>
				</view>
			</van-panel>
		</template>
	</view>
</template>

<script>
	// import Toast from '../../wxcomponents/vant/toast/toast';
	const db = wx.cloud.database()
	export default {
		data() {
			return {
				isLoading: true,
				dataTable:[]
			};
		},
		async onLoad(){
			await wx.cloud.callFunction({
			  name: 'getReply'
			}).then(res => {
				this.dataTable = res.result.result
				this.isLoading = false
			}).catch(err => {
			  // handle error
			})
		},
		methods:{
			toDetails(course, teacher, date, desc){
				uni.navigateTo({
					url:"/pages/reply-details/reply-details?course=" + course +"&teacher=" + teacher + "&date=" + date + "&desc=" + desc
				})
			}
		}
	}
</script>

<style lang="scss">
	.cell-box {
		margin-bottom: 50rpx;
	}
</style>
