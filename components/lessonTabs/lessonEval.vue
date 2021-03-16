<template>
	<view>
		<text>课程评分</text>
		
		<!-- 设置评分数 -->
		<uni-rate :max="5" :value="2.5" size="50" allowHalf="true" @change="onChange"></uni-rate>
		
	</view>
</template>

<script>
	export default {
		data () {
			return {
			}
		},
		methods: {
			onChange(e) {
				console.log('rate发生改变:' + JSON.stringify(e))
			}
		}
	}
</script>

<style>
</style>
