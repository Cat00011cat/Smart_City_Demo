<template>
	<view>
		<view class="loginBox">
			<view class="text">用户名</view>
			<input type="text" v-model="username" />
			<view class="text">密码</view>
			<input type="password" v-model="password" />
			<button type="default" @click="login()">登录</button>
			<button type="default" @click="register()">注册</button>
		</view>
	</view>
</template>

<script>
	const urlHead = getApp().globalData.urlHead;
	export default {
		data() {
			return {
				username: '',
				password: ''
			}
		},
		onLoad() {
			uni.showToast({
				icon: 'error',
				title: '请先登录'
			})
		},
		methods: {
			// 注册页面跳转
			register() {
				uni.navigateTo({
					url: 'register',
				})
			},

			// 登录功能
			login() {
				let that = this;
				let username = that.username;
				let password = that.password;
				// 控制台以JSON方法输出用户的账号和密码.
				// console.log(JSON.stringify());
				console.log(username);
				console.log(password);
				uni.request({
					url: urlHead + "/prod-api/api/login",
					method: "POST",
					data: ({
						"username": username,
						"password": password,
					}),
					// 成功回调函数success
					success: (res) => {
						// 获取服务器返回的数据
						var data = res.data;
						console.log(data);

						// 判断是否登录成功并存储token
						if (data.code == 200) {
							// 数据存储
							var token = res.data.token;
							that.updataToken(token);

						}

					}
				});
			},

			// 更新登录状态
			updataLogin() {
				uni.setStorage({
					key: 'isLogin',
					data: true,
					success() {
						console.log('已登录')
						// 登录成功返回上一层
						uni.navigateBack()
					}
				})
			},

			// 更新Token
			updataToken(token) {
				// 更新token
				let that = this;
				uni.setStorage({
					key: "Authorization",
					data: "Bearer " + token,
					success() {
						console.log("成功存储");
						that.updataLogin()
					}
				})
			}

		}
	}
</script>

<style>
	.text {
		padding: 5px;
	}

	input {
		border-bottom: 1px solid #F2F2F6;
	}
</style>
