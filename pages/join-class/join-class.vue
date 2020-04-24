<template>
	<view>
		<van-form>
			<van-field
			  :value="invite"
			  clearable
			  label="邀请码"
			  placeholder="请输入邀请码"
			  bind:click-icon="onClickIcon"
				@change="inputInvite"
			/>
		</van-form>
		<van-button round block type="info" native-type="submit" @click="onSubmit">
		      提交
		</van-button>
		<van-toast id="van-toast"/>
	</view>
</template>

<script>
	import Toast from '../../wxcomponents/vant/toast/toast';
	export default {
		data() {
			return {
				invite: ''
			};
		},
		methods:{
			async onSubmit(){
				await wx.cloud.callFunction({
				  name: 'joinClass',
				  data: {
					  invite: this.invite
				  }
				}).then(res => {
					if(res.result.result == true){
						uni.redirectTo({
						    url: '/pages/success/success'
						})
					}else{
						Toast('邀请码有误')
					}
				}).catch(err => {
				  // handle error
				})
			},
			inputInvite(event){
				this.invite = event.detail
			},
		}
	}
</script>

<style lang="scss">
	
</style>
