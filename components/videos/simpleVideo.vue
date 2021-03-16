<template>
	<view class="video_box">
		<video class="video_lab" id="zmjVid"
			:src="vidInfo.source[vidType].path"
			autoplay="false"
			controls="false"
			enable-progress-gesture="false"
			:initial-time="vidInfo.record"
			@play="vidPlayCallback"
			@pause="vidPauseCallback"
			@fullscreenchange="screenChange"
			@timeupdate="updateTime"
			@click="clickVideo">
			<!-- title部分 -->
			<view :class="isFullScreen ? 'title_box_full_screen' : 'title_box'" v-if="isControlShow">
				<image class="back_icon" src="../../static/icon_back.png" @click.stop="backClick"></image>
				<view class="title_content" >
					<text class="controls_text">{{vidInfo.title}}</text>
				</view>
			</view>
			
			<!-- 清晰度选择部分 -->
			<view :class="isFullScreen ? 'clear_rwap_full_screen' : 'clear_rwap'" v-if="isSelecting">
				<view v-for="item in vidInfo.source" :key="item.index">
					<view class="clear_item" @click.stop="selectClear(item.type)">{{item.type}}</view>
				</view>
			</view>
			<!-- 底部控制条部分 -->
			<view :class="isFullScreen ? 'bottom_controls_full_screen' : 'bottom_controls'"  v-if="isControlShow">
				<image class="controls_play" buible="true" @click.stop="playOrPause" :src="isPlaying ? '../../static/icon_pause.png' : '../../static/play.png'"></image>
				<text class="controls_text">{{playedTime}}</text>
				<progress class="controls_progress" @click="progressClick" @touchstart="touchStart" @touchend="touchEnd" @touchmove="touchMove" :percent="percent" stroke-width="2" border-radius="10"></progress>
				<text class="controls_text">{{allTime}}</text>
				<view class="control_clear" @click.stop="choseType">
					<text class="controls_text">{{vidInfo.source[vidType].type}}</text>
				</view>
				<image class="control_full_screen" src="../../static/icon_full_screen.png" @click="requestFullScreen"></image>
			</view>
		</video>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				videoContext:{},	//控制video上下文
				isPlaying:false,
				isControlShow:true,		//控制条是否显示
				vidType:0,				//清晰度  默认0：流畅   1：高清  2：原画
				duration:0,				//总时长
				current:0,				//当前时长（为计算百分比默认乘了100）
				percent:0,				//底部时间调
				isFullScreen:false,		//是否为全屏
				isSelecting:false,		//是否正在选择清晰度
				timeOutFun:{},			//延迟执行事件
				progressLength:0,		//progress长度
				touchStartPosition:0,	//
				touchEndPosition:0,		//
				playedTime: '00:00',	//
			}
		},
		props:['vidInfo'],
		watch:{
			//监视isControlShow   超过3秒隐藏控制条
			isControlShow(newState,oldState){
				//console.log('watchisControlShow',newState);
				if(newState && this.isPlaying){	//显示标志
					clearTimeout(this.timeOutFun);
					this.timeOutFun = setTimeout(()=>{
						if(this.isControlShow){
							this.isControlShow = false;
						}
					},5000);
				}
			},
			current(newVal) {
				this.playedTime = this.formateTime(parseInt(newVal/100))
			}
		},
		computed:{
			getScreenWidth:function(){
				return 0;
			},
			// 总时长
			allTime: function () {
				return this.formateTime(365);
			}
		},
		created() {
			this.videoContext = uni.createVideoContext('zmjVid',this);
			console.log('created',this.vidInfo);
			this.current = this.vidInfo.record;
			//this.videoPath = "http://bdmov.a.yximgs.com/bs2/gdtPostRoll/postRoll-MTA3MDk5Mjc5OTg.mp4";
		},
		destroyed() {
			//console.log('视频播放情况：' + this.current + '/' + this.duration + '比例:' + this.current / this.duration);
		},
		methods:{
			vidPlayCallback(e){
				this.isPlaying = true;
			},
			vidPauseCallback(e){
				this.isPlaying = false;
				this.isControlShow = true;
			},
			clickVideo(e){
				console.log('click父Video......');
				this.hideOrShowControlsDelay(300);
			},
			clickCover(e){
				this.isControlShow = !this.isControlShow;
				console.log('isControlShow',this.isControlShow);
			},
			//返回按钮点击
			backClick(e){
				e.stopPropagation();
				console.log('视频播放情况：currentTime:' + this.current + '/duration:' + this.duration + '比例:' + this.current / this.duration);
				if(this.isFullScreen){
					this.videoContext.exitFullScreen();
				}else{
					uni.showToast({
						title:'开始关闭界面',
						icon:'none'
					});
				}
			},
			playOrPause(e){
				e.stopPropagation();
				console.log('clickView子View.......');
				if(this.isPlaying){
					this.videoContext.pause();
				}else{
					this.videoContext.play();
				}
			},
			updateTime(e){
				//console.log('updateTime',e);
				this.duration = parseFloat(e.detail.duration);
				this.playedTime = parseInt(e.detail.currentTime);
				this.current = parseFloat(e.detail.currentTime) * 100;
				this.percent = this.current / this.duration;
				//给父组件发送播放的比例
				// console.log('playedTime', this.playedTime)
				// console.log('video',this.current + '/' + this.duration + '=' + this.percent);
				uni.$emit('playPercent',this.percent);
				//console.log('duration:' + duration + 'current:' + current + 'percent:' + this.percent);
			},
			//选择清晰度
			choseType(){
				uni.showToast({
					title:'切换清晰度',
					icon:'none'
				});
				this.isSelecting = true;
			},
			screenChange(e){
				if(e.detail.fullScreen && e.detail.direction === 'horizontal'){
					this.isFullScreen = true;
				}else{
					this.isFullScreen = false;
				}
				this.isControlShow = true;
			},
			requestFullScreen(){
				if(this.isFullScreen){
					this.videoContext.exitFullScreen();
				}else{
					this.videoContext.requestFullScreen();
				}
			},
			//延迟 显示/隐藏 控制条
			hideOrShowControlsDelay(time){
				setTimeout(()=>{
					this.isControlShow = !this.isControlShow;
				},time);
			},
			//选择清晰度
			selectClear(index){
				switch(index){
					case '流畅':
						this.vidType = 0;
						break;
					case '高清':
						this.vidType = 1;
						break;
					case '原画':
						this.vidType = 2;
						break;
				}
				this.isSelecting = false;
			},
			progressClick(e){
				e.stopPropagation();
				console.log('progressClick',e);
			},
			touchMove(e){
				e.stopPropagation();
				//console.log('progresstouchMove',e);\
			},
			touchStart(e){
				e.stopPropagation();
				console.log('touchStart',e);
			},
			touchEnd(e){
				e.stopPropagation();
				console.log('touchEnd',e);
			},
			// time 单位  秒
			formateTime (time) {
				console.log('currentTime' + time);
				var hour = parseInt(time/3600) < 10 ? '0' + parseInt(time/3600) : parseInt(time/3600);
				time = time %3600;
				
				var mintue = parseInt(time/60) < 10 ? '0' + parseInt(time/60) : parseInt(time/60);
				var second = time % 60 < 10 ? '0' + (time % 60).toString() : (time % 60).toString();
				var timeStr = hour > 0 ? hour.toString() + ':' + mintue.toString() + ':' + second.toString() : mintue.toString() + ':' + second.toString();
				return timeStr;
			}
		}
	}
</script>

<style>
	.video_box{
		
	}
	.video_lab{
		width: 750rpx;
		height: 422rpx;
	}
	.video_title{
		width: 750rpx;
		height: 60rpx;
	}
	/*顶部title*/
	.title_box{
		flex-direction: row;
		width: 750rpx;
		height: 60rpx;
		background-color: #808080;
		opacity: 0.8;
	}
	/*顶部title全屏时显示*/
	.title_box_full_screen{
		flex-direction: row;
		height: 60rpx;
		background-color: #808080;
		opacity: 0.8;
	}
	.back_icon{
		width: 60rpx;
		height: 60rpx;
		padding: 10rpx;
	}
	.title_content{
		flex: 1;
		flex-direction: row;
		align-items: center;
		overflow: hidden;
		text-overflow: ellipsis;
		width: 0;
		height: 60rpx;
		margin-left: 20rpx;
		font-size: 20rpx;
	}
	/*底部控制条*/
	.bottom_controls{
		position: absolute;
		bottom: 0px;
		flex-direction: row;
		width: 750rpx;
		height: 80rpx;
		background-color: #808080;
		opacity: 0.6;
	}
	/*全屏时底部控制条显示*/
	.bottom_controls_full_screen{
		flex-direction: row;
		justify-content: space-between;
		height: 80rpx;
		background-color: #808080;
		opacity: 0.8;
		margin-top: 614rpx;/*750rpx是横屏时的高度，750rpx减去上部title的高度再减去自身的高度就是margin-top的值，这个很关键*/
	}
	.controls_play{
		flex-direction: row;
		width: 60rpx;
		height: 60rpx;
		padding: 10rpx;
		margin: 10rpx;
	}
	.controls_progress{
		flex: 4;
		width: 200rpx;
		height: 80rpx;
		margin-left: 10rpx;
		margin-right: 10rpx;
	}
	.controls_text {
		height: 80rpx;
		line-height: 80rpx;
		font-size: 20rpx;
		color: #FFFFFF;
	}
	.control_clear{
		height: 80rpx;
		flex-direction: row;
		align-items: center;
		margin-left: 10rpx;
	}
	.control_full_screen{
		width: 60rpx;
		height: 60rpx;
		margin: 10rpx;
	}
	/*清晰度选择*/
	.clear_rwap{
		flex-direction: column;
		position: absolute;
		top: 80rpx;
		left: 650rpx;
		width: 100rpx;
		height: 200rpx;
		background-color: #808080;
		opacity: 0.8;
	}
	.clear_rwap_full_screen{
		flex-direction: column;
		position: absolute;
		top: 80rpx;
		right: 0;
		width: 100rpx;
		height: 200rpx;
		background-color: #808080;
		opacity: 0.8;
	}
	/*全屏模式下清晰度选择*/
	.clear_item{
		width: 80rpx;
		margin: 10rpx;
	}
</style>