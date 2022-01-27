<template>
	<view>
		<!-- 首先先创建一个大盒子,然后接着view input button -->
		<view class="regBox">
			<image style="width: 100px; height: 100px;background-color: #b1b1b1; border-radius: 100%;margin: 10px;"
				@click="getavatar()" :src="avatar">
			</image>
			<button size="mini" @click="getavatar()">上传头像</button>
			<input class="input" v-model="avatar" />

			<view class="text">用户名</view>
			<input class="input" v-model="userName" />

			<view class="text">昵称</view>
			<input class="input" v-model="nickName" />

			<view class="text">密码</view>
			<input class="input" type="password" v-model="password" />

			<view class="text">电话号码</view>
			<input class="input" v-model="phonenumber" />

			<view class="text">性别</view>
			<input class="input" v-model="sex" />

			<view class="text">邮箱</view>
			<input class="input" v-model="email" />

			<view class="text">身份证</view>
			<input class="input" v-model="idCard" />

			<button class="text" type="default" @click="reg()">注册</button>

		</view>
	</view>
</template>

<script>
	// 嘀嗒嘀嗒嘀嗒,温馨提示,,在使用网络请求之前必须把,ip拿进来
	const urlHead = getApp().globalData.urlHead;

	export default {
		data() {
			return {
				// 存储获取到本地头像的路径
				avatar: '',
				userName: '',
				nickName: '',
				password: '',
				phonenumber: '',
				sex: '',
				email: '',
				idCard: '',
			}
		},
		methods: {
			// 获取本地头像路径
			getavatar() {
				let that = this;
				uni.chooseImage({
					count: 1, //最多可以选择的图片张数，默认9
					success(res) {
						// console.log(res);
						// console.log(JSON.stringify(res.tempFilePaths));
						that.avatar = res.tempFilePaths[0];
						console.log(that.avatar);
					}
				})
			},

			// post注册信息
			reg() {
				//存储用户输入的数据
				let that = this;
				let avatar = that.avatar;
				let userName = that.userName;
				let nickName = that.nickName;
				let password = that.password;
				let phonenumber = that.phonenumber;
				let sex = that.sex;
				let email = that.email;
				let idCard = that.idCard;
				// 发起网络请求提交数据
				uni.request({
					url: urlHead + '/prod-api/api/register',
					method: 'POST',
					data: ({
						"avatar": avatar,
						"userName": userName,
						"nickName": nickName,
						"password": password,
						"phonenumber": phonenumber,
						"sex": sex,
						"email": email,
						"idCard": idCard,
					}),
					// 请求发起成功回调函数
					success: (res) => {
						var data = res.data;

						// 提示登录成功
						uni.showToast({
							icon: 'success',
							title: data.msg,
						});

						// 使用定时器 三秒执行
						setTimeout(function() {
							uni.navigateBack({
								delta: 1
							})
						}, 3000)


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
		/* 输入框添加下划线 */
		border-bottom: 1px solid #cacacc;
	}
</style>
