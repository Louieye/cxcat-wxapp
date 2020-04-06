<template>
	<view class="body">
		<van-cell-group class="head">
		  <van-cell title="预约课程" :value="tableData.title" />
		  <van-cell title="预约时间" :value="tableData.date" label="更改预约时间请联系客服" />
		</van-cell-group>
		<van-cell-group>
		  <van-cell title="预约状态" :value="tableData.tag == 0?'预约中':'预约成功'" />
		  <van-cell title="老师回复"  :label="tableData.desc" is-link arrow-direction="down" />
		</van-cell-group>
		<view class="button-box">
			<van-button type="primary" block open-type="contact">联系客服</van-button>
			<van-button type="danger" block @click="handleDelete">取消预约</van-button>
		</view>
	</view>
</template>

<script>
	const db = wx.cloud.database();
	export default {
		data() {
			return {
				tableData: {},
				delete: false
			}
		},
		onLoad(option){
			db.collection('appointment').where({_id: option._id}).get().then(res =>{
				this.tableData = res.data[0]
				console.log(this.tableData)
			})
		},
		methods:{
			handleDelete(){
				uni.showModal({
				    title: '提示',
				    content: '确认取消预约？',
				    success:(res) =>{
				        if (res.confirm) {
				            console.log('用户点击确定');
							wx.cloud.callFunction({
							  name: 'removeAppoin',
							  data: {
							    _id: this.tableData._id
							  },
							})
							uni.showToast({
							    title: '取消预约成功',
							    duration: 1500
							})
							setTimeout(()=>{
								uni.switchTab({
								    url: '/pages/user/user'
								});
							},1500)
				        } else if (res.cancel) {
				            console.log('用户点击取消');
				        }
				    }
				});
			},
			deleteAppoin(){
				db.collection('appointment').where({_id: this.tableData._id}).remove().then(res =>{
					console.log(this.tableData)
					uni.showToast({
					    title: '取消预约成功',
					    duration: 2000
					})
					uni.navigateBack({
					    delta: 1
					})
				})
			}
		}
	}
</script>

<style lang="scss">
	.body{
		display: flex;
		flex-direction: column;
	}
	.head{
		margin-bottom: 100rpx;
	}
	.button-box {
		position: absolute;
		left: 50%;
		transform: translate(-50%,0);
		width: 95%;
		.van-button {
			margin-bottom: 20rpx;
		}
		bottom: 0;
	}
</style>
