<template>
  <div class="app">
    <wrap title="基础用法" padding>
      <van-tag class="demo-margin-right">标签</van-tag>
      <van-tag class="demo-margin-right" type="danger">标签</van-tag>
      <van-tag class="demo-margin-right" type="primary">标签</van-tag>
      <van-tag type="success">标签</van-tag>
    </wrap>

    <wrap title="空心样式" padding>
      <van-tag class="demo-margin-right" plain>标签</van-tag>
      <van-tag class="demo-margin-right" plain type="danger">标签</van-tag>
      <van-tag class="demo-margin-right" plain type="primary">标签</van-tag>
      <van-tag plain type="success">标签</van-tag>
    </wrap>

    <wrap title="圆角样式" padding>
      <van-tag class="demo-margin-right" round>标签</van-tag>
      <van-tag class="demo-margin-right" round type="danger">标签</van-tag>
      <van-tag class="demo-margin-right" round type="primary">标签</van-tag>
      <van-tag round type="success">标签</van-tag>
    </wrap>

    <wrap title="标记样式" padding>
      <van-tag class="demo-margin-right" mark>标签</van-tag>
      <van-tag class="demo-margin-right" mark type="danger">标签</van-tag>
      <van-tag class="demo-margin-right" mark type="primary">标签</van-tag>
      <van-tag mark type="success">标签</van-tag>
    </wrap>

    <wrap title="自定义颜色" padding>
      <van-tag class="demo-margin-right" color="#f2826a">标签</van-tag>
      <van-tag class="demo-margin-right" color="#f2826a" plain>标签</van-tag>
      <van-tag class="demo-margin-right" color="#7232dd">标签</van-tag>
      <van-tag color="#7232dd" plain>标签</van-tag>
    </wrap>

    <wrap title="标签大小" padding>
      <van-tag class="demo-margin-right" type="danger">标签</van-tag>
      <van-tag class="demo-margin-right" type="danger" size="medium"
        >标签</van-tag
      >
      <van-tag type="danger" size="large">标签</van-tag>
    </wrap>
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
};
</script>

<style>
.van-tag {
  margin: 5px;
}
</style>
