<template>
	<view>
		<text class="top_line">课程目录</text>
		<view class="collaps">
			<view v-for="(first,fIndex) in accordion" :key="fIndex">
				<view class="hori_box"  @click="operateFirst(first, fIndex)">
					<text class="first_title">{{first.title}}</text>
					<image class="arrow" :src="first.expanded ? '../../static/angle-up.png' : '../../static/angle-down.png'"/>
				</view>
				<view v-if="first.expanded" class="second" v-for="(second,sIndex) in first.secondList" :key="sIndex">
					<view class="hori_box" @click="operateSecond(second, sIndex, fIndex)">
						<text class="second_title">{{second.title}}</text>
						<image class="arrow" :src="second.expanded ? '../../static/angle-up.png' : '../../static/angle-down.png'"/>
					</view>
					<view v-if="second.expanded" class="thrid" v-for="(third,tIndex) in second.thridList" :key="tIndex">
						<view class="hori_box" @click="onClickThrid(third)">
							<text class="thrid_title">{{third.content}}</text>
							<text class="thrid_title">已学20:00/</text>
							<text class="thrid_title">共20:00/</text>
							<text class="thrid_title">进度50%</text>
						</view>
					</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	import uniCollapse from '@/components/collapse/uni-collapse.vue'
	import uniCollapseItem from '@/components/collapse/uni-collapse.vue'
	export default {
		data() {
			return {
				accordion: [{
						id: 0,
						title: '第一章',
						content: '第一章内容',
						expanded: true,
						secondList: [
							{
								title: '第一章第一节',
								content: '第一章第一节内容',
								expanded: true,
								thridList: [
									{
										title: '第一章第一节第一课',
										content: '第一章第一节第一课内容'
									},
									{
										title: '第一章第一节第二课',
										content: '第一章第一节第二课内容'
									}
								]
							},
							{
								title: '第一章第二节',
								content: '第一章第二节内容',
								expanded: false,
								thridList: [
									{
										title: '三级标题文字',
										content: '三级内容'
									},
									{
										title: '三级标题文字',
										content: '三级内容'
									}
								]
							},
						]
					},
					{
						id: 1,
						title: '第二章',
						content: '第二章内容',
						expanded: false,
						secondList: [
							{
								title: '第二章第一节',
								content: '第二章第一节内容',
								expanded: false,
								thridList: [
									{
										title: '第二章第一节第一课',
										content: '第二章第一节第一课内容'
									},
									{
										title: '第二章第一节第二课',
										content: '第二章第一节第二课内容'
									}
								]
							},
							{
								title: '第二章第二节',
								content: '第二章第二节内容',
								expanded: false,
								thridList: [
									{
										title: '第二章第二节第一课',
										content: '第二章第二节第一课内容'
									},
									{
										title: '第二章第二节第二课',
										content: '第二章第二节第二课内容'
									}
								]
							},
						]
					},
					{
						id: 2,
						title: '第三章',
						content: '第三章内容',
						expanded: false,
						secondList: [
							{
								title: '第三章第一节',
								content: '第三章第一节内容',
								expanded: false,
								thridList: [
									{
										title: '第三章第一节第一课',
										content: '第三章第一节第一课内容'
									},
									{
										title: '第三章第一节第二课',
										content: '第三章第一节第二课内容'
									}
								]
							},
							{
								title: '第三章第二节',
								content: '第三章第二节内容',
								expanded: false,
								thridList: [
									{
										title: '第三章第二节第一课',
										content: '第三章第二节第一课内容'
									},
									{
										title: '第三章第二节第二课',
										content: '第三章第二节第二课内容'
									}
								]
							},
						]
					}
				]
			}
		},
		created() {
		},
		methods: {
			operateFirst(first, fIndex){
				this.accordion[fIndex].expanded = !this.accordion[fIndex].expanded;
			},
			operateSecond(second, sIndex, fIndex){
				this.accordion[fIndex].secondList[sIndex].expanded = !this.accordion[fIndex].secondList[sIndex].expanded;
			},
			onClickThrid(item) {
				console.log('onClickThrid', item);
				this.$emit('clickListenItem', item);
			}
		}
		
	}
</script>

<style>
	.top_line {
		border-top: 2rpx;
		border-style: solid;
		border-color: #007AFF;
		margin-left: 10rpx;
		margin-right: 10rpx;
	}
	.magin_left {
		margin-left: 20rpx;
	}
	.collaps {
		background-color: #F5F6F7;
		padding-left: 20rpx;
	}
	.first_title {
		font-size: 40rpx;
	}
	.second_title {
		font-size: 30rpx;
	}
	.thrid_title {
		line-height: 60rpx;
		font-size: 30rpx;
	}
	.hori_box {
		height: 70rpx;
		flex-direction: row;
		justify-content: space-between;
		align-items: center;
	}
	.arrow {
		width: 70rpx;
		height: 70rpx;
		padding: 20rpx;
	}
	.second {
		margin-left: 30rpx;
	}
	.thrid {
		height: 60rpx;
		margin-left: 10rpx;
		border-radius: 6rpx;
		background-color: #FFFFFF;
		margin-top: 10rpx;
		margin-right: 10rpx;
	}
	.lesson {
		
	}
</style>
