<template>
	<view>
		<van-panel title="选择课程分类">
			<van-cell title="scratch" is-link url="/pages/test/test?title=scratch"></van-cell>
			<van-cell title="python" is-link url="/pages/test/test?title=python"></van-cell>
			<van-cell title="C++" is-link url="/pages/test/test?title=C++"></van-cell>
			<van-cell title="其他" is-link url="/pages/test/test?title=其他"></van-cell>
		</van-panel>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				
			};
		}
	}
</script>

<style lang="scss">

</style>
