<template>
	<view class="content">
		<image class="logo" src="/static/logo.png"></image>
		<view class="text-area">
			<text class="title">{{title}}</text>
		</view>
		<zz-video :vidInfo="baseVideoInfo"></zz-video>
		<!-- <zmj-nvue-video :vidInfo="json"></zmj-nvue-video> -->
	</view>
</template>

<script>
	import zzVideo from '@/components/videos/zzVideo.vue'
	import zmjNvueVideo from '../../components/videos/zmjNvueVideo.vue'
	export default {
		components:{
			zmjNvueVideo,
			zzVideo
		},
		data() {
			return {
				title: 'Hello',
				json:[
					{
						type:'流畅',
						title:'女i哦我让你飞鸟日本纳粹女崔你猜呢完成别vu顶底董事局i',
						path:'https://img.cdn.aliyun.dcloud.net.cn/guide/uniapp/%E7%AC%AC1%E8%AE%B2%EF%BC%88uni-app%E4%BA%A7%E5%93%81%E4%BB%8B%E7%BB%8D%EF%BC%89-%20DCloud%E5%AE%98%E6%96%B9%E8%A7%86%E9%A2%91%E6%95%99%E7%A8%8B@20181126-lite.m4v'
					},
					{
						type:'高清',
						title:'女i哦我让你飞鸟日本纳粹女崔你猜呢完成别vu顶底董事局i',
						path:'http://bdmov.a.yximgs.com/bs2/gdtPostRoll/postRoll-MTA3MDk5Mjc5OTg.mp4'
					},
					{
						type:'原画',
						title:'女i哦我让你飞鸟日本纳粹女崔你猜呢完成别vu顶底董事局i',
						path:'https://img.cdn.aliyun.dcloud.net.cn/guide/uniapp/%E7%AC%AC1%E8%AE%B2%EF%BC%88uni-app%E4%BA%A7%E5%93%81%E4%BB%8B%E7%BB%8D%EF%BC%89-%20DCloud%E5%AE%98%E6%96%B9%E8%A7%86%E9%A2%91%E6%95%99%E7%A8%8B@20181126-lite.m4v'
					}
				],
				
				
				// 一个视频的基本信息
				baseVideoInfo: {
					title:'女i哦我让你飞鸟日本纳粹女崔你猜呢完成别vu顶底董事局i',
					record: 10,	// 播放记录 单位 秒s
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
				}
			}
		},
		onLoad() {

		},
		methods: {

		}
	}
</script>

<style>
	.content {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
	}

	.logo {
		height: 200rpx;
		width: 200rpx;
		margin-top: 200rpx;
		margin-left: auto;
		margin-right: auto;
		margin-bottom: 50rpx;
	}

	.text-area {
		display: flex;
		justify-content: center;
	}

	.title {
		font-size: 36rpx;
		color: #8f8f94;
	}
</style>
