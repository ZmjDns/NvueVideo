<script>
	export default {
		onLaunch: function() {
			console.log('App Launch')
		},
		onShow: function() {
			console.log('App Show')
		},
		onHide: function() {
			console.log('App Hide')
		}
	}
</script>

<style>
	/*每个页面公共css */
	.container{
		width: 750rpx;
		height: 100vh;
		/* #ifndef APP-NVUE */
		display: flex;
		/* #endif */
	}
</style>
