<template>
	<view :style="{marginTop: statusBarHeight + 'px'}">
		<zz-video :vidInfo="vidInfo" @onBack="onVideoBack"></zz-video>
		<text class="title">{{vidInfo.title}}</text>
		<view class="line"></view>
		<view class="tabs_contain">
			<uni-segmented-control :current="current" :values="items.map(it=>it.title)"
				@clickItem="onClickItem" style-type="text" active-color="#0055ff">
			</uni-segmented-control>
		</view>
		<view class="line"></view>
		
		<view class="home_content">
			<view v-if="current === 0">
				<lesson-list @clickListenItem="clickListenItem"></lesson-list>
			</view>
			<view v-if="current === 1">
				<lesson-summary></lesson-summary>
			</view>
			<view v-if="current === 2">
				<lesson-eval></lesson-eval>
			</view>
		</view>
	</view>
</template>

<script>
	import zzVideo from '@/components/videos/zzVideo.vue'
	import uniSegmentedControl from "@/components/uni-segmented-control.vue"
	import lessonList from '@/components/lessonTabs/lessonList.vue'
	import lessonSummary from '@/components/lessonTabs/lessonSummary.vue'
	import lessonEval from '@/components/lessonTabs/lessonEval.vue'
	export default {
		components:{
			zzVideo,
			uniSegmentedControl,
			lessonList,
			lessonSummary,
			lessonEval
		},
		onLoad: function(option) {
			console.log('onLoad', option);
		},
		data() {
			return {
				statusBarHeight: 20,
				vidInfo: {
					title:'女i哦我让你飞鸟日本纳粹女崔你猜呢完成别vu顶底董事局i',
					record: 55,	// 播放记录 单位 秒s
					poster: 'https://ss3.bdstatic.com/70cFv8Sh_Q1YnxGkpoWK1HF6hhy/it/u=2043526486,137027039&fm=26&gp=0.jpg',
					// poster: '',
					source: [
						{
							type:'流畅',
							path:'https://img.cdn.aliyun.dcloud.net.cn/guide/uniapp/%E7%AC%AC1%E8%AE%B2%EF%BC%88uni-app%E4%BA%A7%E5%93%81%E4%BB%8B%E7%BB%8D%EF%BC%89-%20DCloud%E5%AE%98%E6%96%B9%E8%A7%86%E9%A2%91%E6%95%99%E7%A8%8B@20181126-lite.m4v'
						},
						{
							type:'高清',
							path:'http://bdmov.a.yximgs.com/bs2/gdtPostRoll/postRoll-MTA3MDk5Mjc5OTg.mp4'
						},
						{
							type:'原画',
							path:'https://img.cdn.aliyun.dcloud.net.cn/guide/uniapp/%E7%AC%AC1%E8%AE%B2%EF%BC%88uni-app%E4%BA%A7%E5%93%81%E4%BB%8B%E7%BB%8D%EF%BC%89-%20DCloud%E5%AE%98%E6%96%B9%E8%A7%86%E9%A2%91%E6%95%99%E7%A8%8B@20181126-lite.m4v'
						}
					],
				},
				items: [
					{title:'目录'},
					{title:'简介'},
					{title:'评分'}
				],
				current: 0
			}
		},
		created() {
			uni.getSystemInfo({
				success: (res) => {
					console.log('getSystemInfo', res);
					this.statusBarHeight = res.statusBarHeight
				}
			})
		},
		methods: {
			onClickItem(e) {
				if (this.current !== e.currentIndex) {
					this.current = e.currentIndex;
				}
			},
			onVideoBack(e) {
				console.log('onVideoBack', e);
			},
			clickListenItem (e) {
				console.log('clickListenItem', e);
			}
		}
	}
</script>

<style>
	.tabs_contain {
		background-color: #FFFFFF;
	}
	.title {
		padding-left: 20rpx;
		padding-right: 20rpx;
		margin-top: 20rpx;
		margin-bottom: 20rpx;
		font-size: 34rpx;
		lines: 1;
		text-overflow: ellipsis;
		flex-wrap: nowrap;
	}
	.line{
		height: 2rpx;
		background-color: #808080;
		margin-top: 5rpx;
		margin-bottom: 5rpx;
		margin-left: 10rpx;
		margin-right: 10rpx;
	}
</style>

