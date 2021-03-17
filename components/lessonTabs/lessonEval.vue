<template>
	<view>
		<text>课程评分</text>
		
		<!-- 设置评分数 -->
		<view class="rate_wrap">
			<uni-rate :max="5" :value="2.5" size="40" allowHalf="true" @change="onChange"></uni-rate>
			
			<view class="input_wrap">
				<textarea class="input_area" v-model="suggest" placeholder="请输入您的意见..."></textarea>
			</view>
		</view>
		
	</view>
</template>

<script>
	export default {
		data () {
			return {
				suggest: ''
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
	.rate_wrap {
		margin-left: 20rpx;
	}
	.input_wrap {
		height: 200rpx;
		margin-left: 20rpx;
		margin-right: 20rpx;
		border-radius: 6rpx;
		background-color: #CCC;
	}
	.input_area {
		height: 200rpx;
		border-radius: 6rpx;
		padding: 4rpx;
		color: #333333;
	}
</style>
