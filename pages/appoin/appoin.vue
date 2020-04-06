<template>
	<view>
		<van-cell-group>
			<van-cell
			title="课程"
			:value="data.title">
			</van-cell>
		  <van-field
		    :value="data.name"
		    required
		    clearable
		    label="姓名"
		    placeholder="请输入姓名"
			:error-message="errMsg"
		    bind:click-icon="onClickIcon"
			@change="inputName"
			@blur="checkName"
		  />
		  <van-field
		    :value="data.childName"
		    clearable
		    label="孩子姓名"
		    placeholder="请输入孩子姓名"
		    bind:click-icon="onClickIcon"
		  	@change="inputChildName"
		  />
		  <van-field
		    :value="data.childAge"
		    clearable
		    label="孩子年龄"
		    placeholder="请输入孩子年龄"
		    bind:click-icon="onClickIcon"
		  	@change="inputChildAge"
		  />
		  <van-field
		    :value="data.phone"
		    label="手机号"
		    placeholder="请输入手机号"
		    required
			:error-message="errorMsg"
			@change="inputPhone"
			@blur="checkPhone"
		  />
		  <van-cell
		    :value="data.date"
			title="到店时间"
			label="请选择到店时间"
			@click="onOpen"
			is-link
			arrow-direction="down"
		  >
		  </van-cell>
		  <van-action-sheet :show="show" @close="onClose">
		    <van-datetime-picker
		      type="date"
		      :value="currentDate"
		      @input="onInput"
		      :min-date="minDate"
			  @confirm="onConfirm"
			  @cancel="onClose"
		    />
		  </van-action-sheet>
		</van-cell-group>
		<van-button 
		class="button" 
		type="info" 
		@click="submit"
		block
		>立 即 预 约</van-button>
		<!-- toast占位符 必须有 -->
		<van-toast id="van-toast"/>
	</view>
</template>

<script>
	import Toast from '../../wxcomponents/vant/toast/toast';
	const db = wx.cloud.database();
	const appointment = db.collection('appointment');
	
	export default {
		data() {
			return {
				data: {
					title: '',
					name: '',
					childName: '',
					childAge: '',
					phone: '',
					date: '',
					tag: 0,
					desc: ''
				},
				show: false,
				errMsg: '',
				errorMsg: '',
				date:'',
				currentDate: new Date().getTime(),
				minDate: new Date().getTime(),
			};
		},
		methods: {
			submit(){
				if(this.data.name == '' || this.data.phone == '' || this.errMsg != '' || this.errorMsg != ''){
					Toast('信息填写不完整')
				}else{
					 Toast('预约成功')
					 const submitData = this.data
					appointment.add({
					  // data 字段表示需新增的 JSON 数据
					  data: submitData
					})
					.then(res => {
					  uni.switchTab({
					      url: '/pages/home/home'
					  })
					})
					this.data = {
						title: '',
						name: '',
						childName: '',
						childAge: '',
						phone: '',
						date: '',
						desc: '',
						tag: 0
					}
				}
			},
			onOpen(){
				this.show = true
			},
			onClose(){
				this.show = false
			},
			onConfirm(){
				this.show = false
				this.data.date = this.formatter(this.currentDate)
			},
			inputName(event){
				this.data.name = event.detail
			},
			inputPhone(event){
				this.data.phone = event.detail
			},
			inputChildName(event){
				this.data.childName = event.detail
			},
			inputChildAge(event){
				this.data.childAge = event.detail
			},
			checkPhone(event){
				const TEL_CHECK = /^[1][3,4,5,6,7,8,9][0-9]{9}$/
				if(TEL_CHECK.test(event.detail.value)){
					this.errorMsg = ''
				}else{
					this.errorMsg = '请输入正确的手机号'
				}
			},
			checkName(event){
				if(event.detail.value == ''){
					this.errMsg = '姓名不能为空'
				}else{
					this.errMsg = ''
				}
			},
			onInput(event) {  
			    this.currentDate = event.detail
			  },
			formatter(date) {
			      const time = new Date(date);
			      const y = time.getFullYear();
			      const m = time.getMonth()+1;
			      const d = time.getDate();
			      return y+'年-'+ m +'月-'+ d + '日'
			      }
		},
		onLoad: function (option) { //option为object类型，会序列化上个页面传递的参数
			        console.log(option.id,option.title); //打印出上个页面传递的参数。
					this.data.title = option.title
		}
	}
</script>

<style lang="scss">
	.button {
		position: absolute;
		transform: translate(-50%,0);
		left: 50%;
		width: 95%;
		bottom: 10rpx;
	}
</style>
