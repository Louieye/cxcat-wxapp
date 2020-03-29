<template>
  <div class="app">
    <van-search
        :value="value"
        show-action
        placeholder="请输入搜索关键词"
        @change="onChange"
        @search="onSearch"
        @clear="onClear"
      />
	<van-toast id="van-toast" />
	<scroll-view scroll-y="true" >
		<view :key="item.id" v-for="item in dataTable" class="card">
			<van-card :tag="item.tag" :desc="item.desc" :title="item.title" :thumb="item.thumb">
				<view slot="footer"><van-button type="info" size="small" round @click="toDetails(item._id)">查 看</van-button></view>
			</van-card>
		</view>
	</scroll-view>
  </div>
</template>

<script>
import Toast from '../../wxcomponents/vant/toast/toast';
const db = wx.cloud.database();
const goods = db.collection('goods');
export default {
  data() {
    return {
      value: '',
	  dataTable: [],
	  data: []
    };
  },
  methods: {
    onChange(event) {
      this.value = event.detail;
    },
    onSearch(event) {
      if (this.value) {
        wx.showToast({
          title: '搜索：' + this.value,
          icon: 'none',
        });
      }
    },
    onCancel() {
      wx.showToast({
        title: '取消',
        icon: 'none'
      });
    },

    onClear() {
      wx.showToast({
        title: '清空',
        icon: 'none',
      });
    },
	toDetails(id){
		uni.navigateTo({
		    url: '/pages/goods-details/goods-details?value=' + id
		})
	},
	setDataTable() {
		  // const check = (item)=>{
			 //  return item.title.includes(this.value) || item.desc.includes(this.value) || item.tag.includes(this.value)
		  // }
		  if(this.value == ''){
			  this.dataTable = []
		  }else{
			const newData = this.data.filter(item => item.title.toLowerCase().includes(this.value.toLowerCase()) || item.tag.toLowerCase().includes(this.value.toLowerCase()) || item.desc.toLowerCase().includes(this.value.toLowerCase()))
			console.log(newData);
			this.dataTable = newData
		  }
	}
  },
  onLoad: function (option) { //option为object类型，会序列化上个页面传递的参数
          console.log(option.value); //打印出上个页面传递的参数。
		  this.value = option.value
		  goods.get().then(res => {
		    // res.data 包含该记录的数据
		    this.data = res.data
			this.setDataTable()
		  })
      },
	  watch: {
		  value(){
			  this.setDataTable()
		  }
	  },
  components: {
  }
};
</script>

<style>
.demo-radio-group {
  padding: 0 17px;
}

.demo-radio {
  margin-bottom: 10px;
}
</style>
