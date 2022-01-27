<script>
	export default {
		// 定义全局变量
		globalData: {
			// 用于存储用户是否登录的信息
			isLogin:false,
			// 用于存储服务器IP
			urlHead: "http://124.93.196.45:10001"
		},
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
	page {
		background-color: #FFFFFF;
	}
</style>
