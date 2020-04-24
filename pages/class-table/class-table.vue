<template>
	<view>
		<van-tabs v-model="active">
		  <van-tab title="今日课程">
			<van-loading v-if="isLoading" style="margin-left: 45%;" />
			<text v-if="noTodayClass" class="class-text">暂无课程</text>
			<van-cell-group :key="item.id" v-for="item in todayClass">
			  <van-cell :title="item.name" :value="'时间: ' + item.time" :label="'教师：'+ item.teacher + ' 上课地点：' + item.classroom"/>
			</van-cell-group>
		  </van-tab>
		  <van-tab title="全部课程">
			<van-loading v-if="isLoading" style="margin-left: 45%;" />
			<text v-if="noClass" class="class-text">暂无课程</text>
			  <van-list>
			    <van-cell v-for="item in classTable" :key="item.day" :title="item.name" :value="'周' + (item.day==0?'日':item.day) + ' ' + item.time" />
			  </van-list>
		  </van-tab>
		</van-tabs>
	</view>
</template>

<script>
	const db = wx.cloud.database();
	import Toast from '../../wxcomponents/vant/toast/toast';
	export default {
		data() {
			return {
				isLoading: true,
				active: 1,
				classTable: [],
				todayClass: [],
				list: [],
				noTodayClass: false,
				noClass: false
			};
		},
		async mounted(){
			await wx.cloud.callFunction({
			  // 要调用的云函数名称
			  name: 'getClassTable'
			}).then(res => {
			  // this.list = res.result.data.classId
			  this.list = res.result.result.data[0].classId
			  if(this.list.length == 0){
				  this.noClass = true
			  }
			}).catch(err => {
			  // handle error
			})
			for(let i = 0; i < this.list.length; i++){
				await db.collection('classTable').where({
				  id: this.list[i]
				}).get().then(res => {
					this.classTable.push(...res.data)
				})
			}
			const now = new Date().getDay()
			this.todayClass = this.classTable.filter(item => item.day == now)
			this.isLoading = false
			if(this.todayClass.length == 0){
				this.noTodayClass = true
			}
		}
	}
</script>

<style lang="scss">
	.class-text {
		position: relative;
		left: 41%;
		color: gray;
	}
</style>
