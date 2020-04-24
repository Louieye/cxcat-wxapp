<template>
	<view class="main">
		<van-cell-group class="cell-box">
		<van-cell>
		  <view slot="title">
			  <image :src="dataTable.thumb" mode="aspectFit"></image>
		    <view class="van-cell-text">{{dataTable.title}}</view>
		    <van-tag type="danger">{{dataTable.tag}}</van-tag>
		  </view>
		</van-cell>
		<van-cell>
			<view class="price">
				售价：¥ {{dataTable.price || 0}}
			</view>
		</van-cell>
		</van-cell-group>
		<van-cell-group>
			<van-cell>
				<van-tabs :active="active" bind:change="onChange" class="tab" sticky>
				  <van-tab title="介绍">
					  <view class="list">
					  	{{dataTable.details}}
					  </view>
					  <image src="cloud://lyj-app.6c79-lyj-app-1301672818/goodsImg/demo1-3.jpg" mode="aspectFill"></image>
					  <image src="cloud://lyj-app.6c79-lyj-app-1301672818/goodsImg/demo1-1.jpg" mode="aspectFill"></image>
					  <image src="cloud://lyj-app.6c79-lyj-app-1301672818/goodsImg/demo1-2.jpg" mode="aspectFill"></image>
				  </van-tab>
				  <van-tab title="目录">
					  <view v-for="item in dataTable.list" class="list">
						  <text>{{item}}</text>
					  </view>
				  </van-tab>
				  <van-tab title="授课教师">
					  <view class="list">{{dataTable.teacher}}</view>
					  <view class="list">{{dataTable.teacher_d}}</view>
				  </van-tab>
				</van-tabs>
			</van-cell>
		</van-cell-group>
		<van-cell>
			<van-goods-action class="bottom">
			  <van-goods-action-icon
			    icon="chat-o"
			    text="客服"
				open-type="contact"
			    bind:click="onClickIcon"
			  />
			  <van-goods-action-icon
			    icon="cart-o"
			    text="购物车"
			    bind:click="onClickIcon"
			  />
			  <van-goods-action-button
			    text="立即预约"
			    @click="onClickButton"
			  />
			</van-goods-action>
		</van-cell>
	</view>
</template>

<script>
	const db = wx.cloud.database();
	const goods = db.collection('goods');
	import Toast from '../../wxcomponents/vant/toast/toast';
	export default {
		data() {
			return {
				dataTable: {},
				active: 0
			};
		},
		methods:{
			onChange(event) {
				
			},
			 onClickIcon() {
			     Toast('点击图标');
			   },
			 
			   onClickButton() {
			     Toast('点击按钮');
				 uni.navigateTo({
				     url: '/pages/appoin/appoin?id=' + this.dataTable._id + '&title=' + this.dataTable.title
				 })
			   }
		},
		onLoad: function (option) { //option为object类型，会序列化上个页面传递的参数
				  goods.doc(option.value).get().then(res => {
				    // res.data 包含该记录的数据
				    this.dataTable = res.data
				  })
	}
}
</script>

<style lang="scss" scoped>
	.price {
		text-align: left;
		font-size: 35rpx;
		font-weight: bold;
		color: rgb(255,68,68);
		margin-bottom: 20rpx;
	} 
	.tab {
		width: 100%;
		image {
			margin: 0;
			padding: 0;
			width: 100%;
		}
	}
	.main {
		background-color: rgb(248, 248, 248);
		display: flex;
		flex-direction: column;
		justify-content: space-around;
	}
	.list {
		margin-top: 50rpx;
		text-align: left;
	}
	.cell-box {
		margin-bottom: 100rpx;
	}
	.bottom {
		position: absolute;
		z-index: 999;
	}
</style>
