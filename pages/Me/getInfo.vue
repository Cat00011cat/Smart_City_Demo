<template>
	<view>

		<view class="infoBox">
			<view class="text">用户名</view>
			<input class="input" v-model="userName"></input>

			<view class="text">昵称</view>
			<input class="input" v-model="nickName"></input>

			<view class="text">电话号码</view>
			<input class="input" v-model="phonenumber"></input>

			<view class="text">性别</view>
			<input class="input" v-model="sex"></input>

			<view class="text">邮箱</view>
			<input class="input" v-model="email"></input>

			<view class="text">身份证</view>
			<input class="input" v-model="idCard"></input>
		</view>
		<button @click="reset()">修改</button>

	</view>
</template>

<script>
	// 定义服务器IP
	const urlHead = getApp().globalData.urlHead;

	export default {
		data() {
			return {
				// 存储用户输入的数据
				avatar: '',
				userName: '',
				nickName: '',
				phonenumber: '',
				sex: '',
				email: '',
				idCard: '',
			}
		},
		onShow() {
			this.getUserInfo()
		},
		methods: {
			// 修改用户信息
			reset() {
				let that = this;
				uni.request({
					url: urlHead + '/prod-api/api/common/user',
					method: 'PUT',
					header: {
						'Authorization': uni.getStorageSync('Authorization')
					},
					data: {
						'email': that.email,
						'idCard': that.idCard,
						'nickName': that.nickName,
						'phonenumber': that.phonenumber,
						'sex': that.sex,
					},

					// 成功回调函数
					success(res) {
						var data = res.data;
						console.log(data);

						uni.showToast({
							icon: 'success',
							title: data.msg,
						})

					}

				})
			},


			// 如果用户没有登录则跳转登录页面
			jumpToLogin() {
				// 登录跳转
				this.isLogin = uni.getStorageSync("isLogin")
				if (this.isLogin) {
					return
				}
				console.log("未登录");
				uni.navigateTo({
					url: "login"
				})
			},

			// 获取个人信息
			getUserInfo() {
				let that = this;
				uni.request({
					url: urlHead + '/prod-api/api/common/user/getInfo',
					method: 'GET',
					header: {
						"Authorization": uni.getStorageSync("Authorization")
					},
					// 成功回调函数
					success(res) {
						var data = res.data;
						// 以json格式化  打印数据
						console.log(JSON.stringify(data));

						// 判断是否登录
						if (data.code == 401) {
							uni.setStorage({
								key: 'isLogin',
								data: false
							})

							// 调用方法
							that.jumpToLogin()

						}
						// 登录成功则存储用户信息
						// that.userInfo = data.user;
						that.userName = data.user.userName;
						that.nickName = data.user.nickName;
						that.phonenumber = data.user.phonenumber;
						// 性别判断0渲染男 1渲染女
						if (data.user.sex == 0) {
							that.sex = '男'
						} else {
							that.sex = '女'
						}
						that.email = data.user.email;
						that.idCard = data.user.idCard;

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
		border-bottom: 1px solid #cacacc;
	}
</style>
