<template>
	<view>
		<view class="mineBackground">
			<view class="mineBackground">
				<image class="mineAvatar" :src="getImageUrl(userInfo.avatar)" @click="loginPage()"></image>
				<text class=" mineName" @click="loginPage()" v-text="userInfo.nickName">{{userName}}</text>
			</view>
			<view class="mineMenu">

				<view class="meTitle" @click="getInfo()">
					<text>个人信息</text>
					<text class="right">></text>
				</view>

				<view class="meTitle" @click="orderlist()">
					<text>订单列表</text>
					<text class="right">></text>
				</view>

				<view class="meTitle" @click="resetPassword()">
					<text>修改密码</text>
					<text class="right">></text>
				</view>

				<view class="meTitle" @click="feedback()">
					<text>意见反馈</text>
					<text class="right">></text>
				</view>

				<view class="meTitle" @click="loginout()">
					<button type="warn" plain="true">退出登录</button>
				</view>

			</view>
		</view>
	</view>
</template>

<script>
	const urlHead = getApp().globalData.urlHead;
	export default {
		onLoad() {},
		data() {
			return {
				userName: '立即登录',
				userInfo: []
			}
		},
		onShow() {
			this.getUserInfo()
		},
		methods: {

			// 意见反馈模块跳转
			feedback() {
				uni.navigateTo({
					url: 'feedback'
				})
			},

			// 订单列表跳转
			orderlist() {
				uni.navigateTo({
					url: 'orderlist'
				})
			},

			// 登录跳转
			jumpToLogin() {
				this.isLogin = uni.getStorageSync("isLogin")
				if (this.isLogin) {
					return
				}
				console.log("未登录");
				uni.navigateTo({
					url: "login"
				})
			},

			//获取个人信息
			getUserInfo() {
				let that = this
				uni.request({
					url: urlHead + "/prod-api/api/common/user/getInfo",
					method: "GET",
					header: {
						"Authorization": uni.getStorageSync("Authorization")
					},
					success(res) {
						var data = res.data
						console.log(JSON.stringify(data));
						if (data.code == 401) {
							uni.setStorage({
								key: "isLogin",
								data: false
							})
							that.jumpToLogin()
						}
						that.userInfo = data.user
					}
				})
			},

			// 各个页面的跳转
			getInfo: function() {
				uni.navigateTo({
					url: 'getInfo'
				})
				console.log("infoSuccess")
			},
			loginPage: function() {
				uni.navigateTo({
					url: 'login'
				})
				console.log('loginPageSuccess')
			},
			resetPassword: function() {
				uni.navigateTo({
					url: 'resetpassword'
				})
				console.log("resetpasswordSuccess")
			},
			loginout: function() {
				uni.request({
					url: urlHead + '/logout',
					success(res) {
						console.log(res.data);
						uni.showToast({
							icon: 'success',
							title: '退出成功'
						})
					}
				})
			},

			// 图片显示
			getImageUrl(index) {
				if (index == null) {
					return
				}
				if (index.search("/prod-api") == -1) {
					index = "/prod-api" + index
				}
				return urlHead + index
			}
		}
	}
</script>

<style>
	.mineBackground {
		height: 100px;
		margin: 30px 10px 10px 10px;
	}

	.mineAvatar {
		float: left;
		width: 88px;
		height: 88px;
		top: 5px;
		border-radius: 100%;
	}

	.mineName {
		font-size: 18px;
		float: left;
		margin: 30px 30px 30px 20px;
	}

	.loginout {
		padding: 120px;
	}

	.meTitle {
		padding: 50rpx;
		/* background-color: #FFFFFF; */
	}

	.right {
		float: right;
	}
</style>
