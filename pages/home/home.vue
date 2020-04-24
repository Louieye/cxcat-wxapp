<template>
	<scroll-view class="content" @scrolltolower="scrollToLower">
		<van-search
		    :value="value"
		    show-action
		    placeholder="请输入搜索关键词"
		    @change="onChange"
		    @search="onSearch"
		    @cancel="onCancel"
		    @clear="onClear"
			shape="round"
		  />
		<swiper :indicator-dots="true" :autoplay="true" :interval="3000" :duration="1000">
			<swiper-item><image src="../../static/img/lunbo1.jpg" mode="aspectFill"></image></swiper-item>
			<swiper-item><image src="../../static/img/lunbo6.jpg" mode="aspectFill"></image></swiper-item>
			<swiper-item><image src="../../static/img/lunbo2.jpg" mode="aspectFill"></image></swiper-item>
			<swiper-item><image src="../../static/img/lunbo5.jpg" mode="aspectFill"></image></swiper-item>
			<swiper-item><image src="../../static/img/lunbo4.jpg" mode="aspectFill"></image></swiper-item>
		</swiper>
		<view class="main">
			<text>在教课程</text>
			<view :key="item._id" v-for="item in dataTable" class="card">
				<van-card :tag="item.tag" :desc="item.desc" :title="item.title" :thumb="item.thumb">
					<view slot="footer"><van-button type="info" size="small" round @click="toDetails(item._id)">查 看</van-button></view>
				</van-card>
			</view>
		</view>
	</scroll-view>
</template>

<script>

const db = wx.cloud.database();
const goods = db.collection('goods');
import Toast from '../../wxcomponents/vant/toast/toast';
export default {
	data() {
		return {
			value: '',
			dataTable: []
		};
	},
	methods: {
		onChange(event) {
		  this.value = event.detail;
		},
		onSearch(event) {
			uni.navigateTo({
			    url: '/pages/search/search?value=' + this.value
			})
		},
		toDetails(id){
			uni.navigateTo({
			    url: '/pages/goods-details/goods-details?value=' + id
			})
		}
	},
	mounted() {
		wx.showLoading({
		  title: '加载中...',
		})
		db.collection('goods').get().then(res => {
			// res.data 包含该记录的数据
			this.dataTable = res.data;
			console.log(res);
		});
		wx.cloud.callFunction({
			  // 云函数名称
			  name: 'doLogin'
			  // 传给云函数的参数
			})
			.then(res => {
			  uni.setStorageSync('openid',res.result.openid)
			  console.log('openid',wx.getStorageSync('openid'))
			  wx.hideLoading()
			})
			.catch(console.error)
		}
	}
</script>

<style lang="scss">
.content {
	swiper {
		height: 380rpx;
		margin-bottom: 20rpx;
	}
	image {
		width: 100%;
	}
}
.main {
	text {
		font-size: 35rpx;
		color: rgb(63, 63, 63);
		font-weight: 550;
		margin-left: 20rpx;
	}
}
</style>
