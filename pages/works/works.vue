<template>
	<view>
		<view :key="item._id" v-for="item in imgList" class="img-box">
			<view class="img">
				<image
				:src="item.fileID"
				/>
				<text>作者：{{item.nickName}}</text>
			</view>
		</view>
	</view>
</template>

<script>
	const db = wx.cloud.database()
	export default {
		data() {
			return {
				imgList: []
			};
		},
		async onLoad() {
			wx.showLoading({
			  title: '加载中...',
			});
			await db.collection('works').get().then(res => {
			  console.log(res.data)
			  this.imgList = res.data
			}).catch(error => {
			  // handle error
			})
			wx.hideLoading()
		},
		methods:{
			getBase64ImageUrl(data) {
			    // 获取到base64Data
			    var base64Data = data;
			    /// 通过微信小程序自带方法将base64转为二进制去除特殊符号，再转回base64
			    base64Data = wx.arrayBufferToBase64(wx.base64ToArrayBuffer(base64Data));
			    /// 拼接请求头，data格式可以为image/png或者image/jpeg等，看需求
			    const base64ImgUrl = "data:image/jpg;base64," + base64Data;
			    /// 刷新数据
			   console.log(base64ImgUrl)
			    return base64ImgUrl
			  }
		}
	}
</script>

<style lang="scss">
	.img-box {
		width: 100%;
		display: flex;
		flex-direction: column;
		align-items: center;
	}
	.img {
		margin-bottom: 30rpx;
		display: flex;
		flex-direction: column;
		align-items: center;
		border: 1rpx soild gray;
		border-radius: 5rpx;
		box-shadow: 0 0 1rpx 1rpx rgba($color: #bfbfbf, $alpha: 0.5);
	}
</style>
