<template>
	<view class="main">
		<view class="head">
			<van-nav-bar
			  :title="'第' + currentIndex + '题'"
			  :left-text="currentIndex + '/' + totalIndex"
			  right-text="选题"
			  @click-right="show = true"
			/>
		</view>
		<view class="qusetion">
			<van-cell title="问题:" :label="dataTable[currentIndex - 1].question" :value="correctAnswer[currentIndex - 1].length == 1?'单选':'多选'"></van-cell>
		</view>
		<view class="select">
			<van-checkbox-group :value="form" data-key="form" @change="onChange">
				<van-cell-group>
					<van-cell v-for="(item,index) in list" :key="index" :title="'选项'+item.index" :label="item.value" value-class="value-class" clickable
					 :data-name="item.index" @click="toggle">
						<van-checkbox @tap.native.stop="noop"  :class="'checkboxes-'+item.index" :name="item.index" /><!-- TODO 这个catch:tap="noop"会打告警 -->
					</van-cell>
				</van-cell-group>
			</van-checkbox-group>
		</view>
		<view class="footer">
			<van-button block @click="lastQuestion">上一题</van-button>
			<van-button block type="info" @click="nextQuestion">下一题</van-button>
			<van-button block type="primary" @click="handleSubmit">交卷</van-button>
		</view>
		<van-action-sheet :show="show" :actions="actions" @select="onSelect" cancel-text="取消" @cancel="onCancel" />
		<van-toast id="van-toast"/>
	</view>
</template>

<script>
	import Toast from '../../wxcomponents/vant/toast/toast';
	const db = wx.cloud.database();
	export default {
		data() {
			return {
				currentIndex: 1,
				totalIndex: 1,
				list:[],
				dataTable: [],
				form:[],
				answer: {},
				correctAnswer: [],
				actions: [
				    { name: '选项' },
				    { name: '选项' },
				    { name: '选项', subname: '描述信息' },
				],
				show: false
			};
		},
		async onLoad(option){ //option为object类型，会序列化上个页面传递的参数
			wx.showLoading({
				title: '加载中...',
			});
			await wx.cloud.callFunction({
			  name: 'getTest',
			  data: {
				  title: option.title
			  }
			}).then(res => {
				this.dataTable = res.result.result
				console.log('dataTable', this.dataTable);
				this.list = this.dataTable[0].list
				this.totalIndex = this.dataTable.length
				for(let i = 0; i < this.dataTable.length; i++){
					this.correctAnswer.push(this.dataTable[i].answer)
				}
				wx.hideLoading()
			}).catch(err => {
			  console.log(err);
			})
			
		},
		methods:{
			onSelect(item) {
			      // 默认情况下点击选项时不会自动收起
			      // 可以通过 close-on-click-action 属性开启自动收起
			      this.show = false;
			    },
			onCancel() {
			    this.show = false;
			},
			onChange(event) {
				const {
					key
				} = event.currentTarget.dataset;
				this[key] = event.detail
			},
			toggle(event) {
				const {
					name
				} = event.currentTarget.dataset;
				const checkbox = this.selectComponent(`.checkboxes-${name}`);
				checkbox.toggle();
			},
			noop() {},
			lastQuestion(){
				if(this.currentIndex > 1){
					this.currentIndex--
				}else{
					Toast('前面没有了')
				}
			},
			nextQuestion(){
				if(this.currentIndex < this.totalIndex){
					this.currentIndex++
				}else{
					Toast('已经是最后一题了，请交卷')
				}
			},
			handleSubmit(){
				this.answer[this.currentIndex] = this.form
				console.log('answer', this.answer);
				console.log('correctAnswer', this.correctAnswer);
				let result = 0
				for(let i = 0; i < this.correctAnswer.length; i++){
					if(this.answer[i + 1] && this.answer[i + 1].join() == this.correctAnswer[i].join()){
						result++
					}
				}
				uni.redirectTo({
					url:"/pages/test-result/test-result?result=" + parseInt(result/this.totalIndex*100)
				})
			}
		},
		watch:{
			currentIndex(newI, oldI){
				this.list = this.dataTable[newI - 1].list
				this.answer[oldI] = this.form
				if(this.answer[newI] && this.answer[newI] != 'undefind'){
					this.form = this.answer[newI]
				}else{
					this.form = []
				}
			}
		}
	}
</script>

<style lang="scss">
	.head {
		width: 100%;
	}
	.question {
		padding-bottom: 50rpx;
		width: 100%;
		van-cell {
			width: 100%;
		}
	}
	.select {
		width: 100%;
		padding-top: 20rpx;
	}
	.footer {
		width: 100%;
		position: absolute;
		bottom: 0;
	}
</style>
