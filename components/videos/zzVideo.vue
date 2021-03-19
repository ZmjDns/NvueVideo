<template>
	<view>
		<video class="video_lab"
			id="videoComp"
			:src="vidInfo.source[type].path"
			:initial-time="initialTime"
			:autoplay="autoplay"
			objectFit="fill"
			controls="false"
			:poster="vidInfo.poster"
			enable-progress-gesture="false"
			@loadedmetadata="loadedmetadata"
			@waiting="onWaiting"
			@play="vidPlayCallback"
			@pause="vidPauseCallback"
			@ended="vidEndCallback"
			@timeupdate="vidTimeUpdate"
			@fullscreenchange="fullScreenChange"
			@click="clickVideo">
			<!-- 顶部标题 -->
			<view v-if="isShowControls" class="control_bg title_box">
				<view class="icon_box" @click="clickBack"><image class="back_icon" src="../../static/icon_back.png"></image></view>
				<text class="text">{{vidInfo.title}}</text>
			</view>
			<!-- 清晰度列表 -->
			<view v-if="isShowClearList" class="control_bg clear_box">
				<text class="text margin" v-for="(item, index) in vidInfo.source" :key="index" @click="onChooseType($event,index)">{{item.type}}</text>
			</view>
			<!-- 底部控制 -->
			<view v-if="isShowControls" class="control_bg controls_box">
				<view class="icon_box" @click="playOrPause">
					<image class="back_icon" :src="isPlaying ? '../../static/icon_pause.png' : '../../static/play.png'"></image>
				</view>
				<text class="text">{{formatTime(currentTime)}}</text>
				<progress class="controls_progress" :percent="percent" stroke-width="2" border-radius="10"></progress>
				<text class="text">{{formatTime(duration)}}</text>
				<text class="text margin" @click="chooseClearType">{{vidType}}</text>
				<view class="back_icon" @click="requestFullScreen">
					<image class="back_icon" src="../../static/icon_full_screen.png"></image>
				</view>
			</view>
		</video>
	</view>
</template>

<script>
	export default {
		props:{
			vidInfo: Object,
			default: () => {}
		},
		data () {
			return {
				videoContext: {},
				autoplay: false,		//	自动播放， 刚进来不必自动播放，切换清晰度之后设置自动播放
				type: 0,				//	清晰度  默认0：流畅   1：高清  2：原画
				isPlaying: false,		//	正在播放
				initialTime: 0,			//	刚进来播放的位置，或者切换清晰度时上衣次播放的位置
				duration: 0,			//	总时长		统一时间单位   s->秒
				currentTime: 0,			//	当前播放时间
				percent: 0,				//	底部时间条
				vidType:'默认',			//	清晰度  默认0：流畅   1：高清  2：原画
				isShowControls: true,	//	是否显示控制条
				isFullScreen: false,	//	是否全屏
				isShowClearList: false,	//	是否显示清晰度列表
				isEnded: false,			//	是否播放完毕
			}
		},
		created() {
			this.videoContext = uni.createVideoContext('videoComp', this);
			console.log('vidINfo', this.vidInfo);
			this.initialTime = this.vidInfo.record;
			this.currentTime = this.vidInfo.record;
			this.vidType = this.vidInfo.source[this.type].type;
		},
		//	销毁前保存播放记录
		beforeDestroy() {
			console.log('销毁之前保存记录：' + this.currentTime.toString());
			this.$emit('onBack', {current: this.currentTime, aa: this.vidInfo});
		},
		methods:{
			loadedmetadata(e) {
				console.log('loadedmetadata', e);
			},
			onWaiting (e) {	//	初始或缓冲时触发
				console.log('onWaiting', e);
			},
			vidPlayCallback(e) {	//	播放
				console.log('vidPlayCallback', e);
				this.isPlaying = true;
			},
			vidPauseCallback(e) {	//	暂停
				console.log('vidPauseCallback', e);
				this.isPlaying = false;
			},
			vidEndCallback(e) {	//	结束
				uni.showToast({
					title:'播放完毕',
					duration: 2000,
					icon:'none'
				})
				this.isEnded = true;
			},
			vidTimeUpdate(e) {	//	播放时间变化
				if (this.duration <= 0) {
					this.duration = e.detail.duration;
				}
				this.currentTime = parseInt(e.detail.currentTime);
				this.percent = e.detail.currentTime/this.duration * 100;
				console.log('this.percent',e.detail.currentTime.toString() + '/' + this.duration.toString() + '=' +  this.percent.toString());
			},
			clickVideo (e) {	//	点击屏幕时显示或隐藏控制条
				console.log('clickVideo父标签', e);
				this.isShowControls = !this.isShowControls;
				console.log('this.isShowControls', this.isShowControls);
				if(!this.isShowControls) {
					this.isShowClearList = false;
				}
			},
			playOrPause(e) {	//	点击播放或暂停
				e.stopPropagation();
				console.log('playOrPause子标签', e);
				if(this.isEnded) {	// 播放完毕，重新播放
					this.replay();
					return;		//	返回出去
				}
				if(this.isPlaying) {
					this.videoContext.pause();
				}else {
					this.videoContext.play();
				}
			},
			requestFullScreen () {		//	全屏或半屏
				if (this.isFullScreen) {
					this.videoContext.exitFullScreen();
				} else {
					this.videoContext.requestFullScreen();
				}
			},
			fullScreenChange(e) {	//	全屏或半屏状态监听
				console.log('fullScreenChange', e);
				if(e.detail.fullScreen && e.detail.direction === 'horizontal') {
					this.isFullScreen = true;
				} else {
					this.isFullScreen = false;
				}
			},
			chooseClearType (e) {	//	显示清晰度
				e.stopPropagation();
				if(!this.isShowClearList) {
					this.isShowClearList = true;
				}
			},
			onChooseType (e,index) {	//	选择的清晰度
				e.stopPropagation();
				console.log('onChooseType', this.vidInfo.source[index]);
				this.type = index;
				this.vidType = this.vidInfo.source[index].type;
				this.autoplay = true;	//	切换清晰度之后设置自动播放
				if (this.currentTime > 0) {
					this.initialTime = this.currentTime
				}
			},
			clickBack (e) {	//	点击title返回箭头
				e.stopPropagation();
				if (this.isFullScreen) {
					this.requestFullScreen();
				} else {
					uni.navigateBack({
						delta: 2
					});
				}
			},
			formatTime(time) {
				time = parseInt(time);
				// console.log('传来时间：', time.toString());
				var timeStr = '';
				var hour = parseInt(time / 3600);
				if (hour > 0) {
					timeStr += hour < 10 ? ('0' + hour.toString()) : hour.toString();
					timeStr += ':'
				}
				var leftMinutes = parseInt(time % 3600);
				var minutes = parseInt(leftMinutes / 60);
				timeStr += minutes < 10 ? ('0' + minutes.toString()) : minutes.toString();
				timeStr += ':';
				var seconds = parseInt(leftMinutes % 60);
				timeStr += seconds < 10 ? ('0' + seconds.toString()) : seconds.toString();
				// console.log('timeStr', timeStr);
				return timeStr;
			},
			//	重头开始播放
			replay () {
				this.videoContext.seek(0);
				this.currentTime = 0;
				this.isEnded = false;
				this.videoContext.play();
			}
		}
	}
</script>

<style>
	.video_lab{
		width: 750rpx;
		height: 422rpx;
	}
	.control_bg {
		background-color: #808080;
		opacity: 0.8;
	}
	.text {
		line-height: 60rpx;
		color: #FFFFFF;
		font-size: 30rpx;
	}
	.title_box {
		position: absolute;
		top: 0;
		left: 0;
		right: 0;
		height: 60rpx;
		flex-direction: row;
		align-items: center;
	}
	.icon_box {
		width: 60rpx;
		height: 60rpx;
		padding-top: 10rpx;
		padding-bottom: 10rpx;
		padding-left: 10rpx;
		padding-right: 10rpx;
	}
	.back_icon{
		width: 40rpx;
		height: 40rpx;
	}
	.clear_box {
		position: absolute;
		top: 100rpx;
		right: 0;
	}
	.controls_box {
		position: absolute;
		left: 0;
		bottom: 0;
		right: 0;
		height: 60rpx;
		flex-direction: row;
		align-items: center;
		padding-right: 10rpx;
	}
	.controls_progress{
		flex: 1;
		width: 200rpx;
		height: 60rpx;
		margin-left: 10rpx;
		margin-right: 10rpx;
	}
	.margin {
		margin-left: 10rpx;
		margin-right: 10rpx;
	}
</style>
