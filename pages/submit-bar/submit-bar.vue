<template>
  <div class="submit-wapper">
    <wrap title="基础用法">
      <van-submit-bar
        custom-class="root-class"
        :price="3050"
        button-text="提交订单"
        @submit="onSubmit"
      />
    </wrap>
    <wrap title="禁用状态">
      <van-submit-bar
        custom-class="root-class"
        disabled
        :price="3050"
        button-text="提交订单"
        tip="您的收货地址不支持同城送, 我们已为您推荐快递"
        @submit="onSubmit"
      />
    </wrap>
    <wrap title="加载状态">
      <van-submit-bar
        custom-class="root-class"
        loading
        :price="3050"
        button-text="提交订单"
        @submit="onSubmit"
        :tip="true"
      />
    </wrap>
    <van-submit-bar
      custom-class="root-class"
      :price="3050"
      button-text="提交订单"
      @submit="onSubmit"
    >
      <van-tag type="primary">标签</van-tag>
      <div slot="tip">您的收货地址不支持同城送, <span>修改地址</span></div>
    </van-submit-bar>
    <van-toast id="van-toast" />
  </div>
</template>
<script>
import wrap from '@/components/wrap';
import Toast from '../../wxcomponents/vant/toast/toast';
export default {
  components: {
    wrap,
  },
  methods: {
    onSubmit() {
      Toast('点击按钮');
    },
  },
};
</script>
<style>
.submit-wapper .root-class {
  position: static;
}
</style>
