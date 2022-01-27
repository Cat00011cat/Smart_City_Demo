<template>
	<view>
		<view>
			<view class="text">用户旧密码</view>
			<input v-model="oldPassword" />
			<view class="text">用户新密码</view>
			<input v-model="newPassword" type="password" />
			<button @click="reset()">修改</button>
		</view>
	</view>
</template>

<script>
	// 嘀嗒嘀嗒，在发起网络请求的时候别忘了 ，把IP拿进来。

	const urlHead = getApp().globalData.urlHead;

	export default {
		data() {
			return {
				oldPassword: '',
				newPassword: '',
			}
		},
		methods: {
			reset() {
				let that = this;
				console.log(that.oldPassword);
				console.log(that.newPassword);

				// 发情网络请求
				uni.request({
					url: urlHead + '/prod-api/api/common/user/resetPwd',
					method: 'PUT',
					header: {
						'Authorization': uni.getStorageSync('Authorization'),
					},
					data: {
						"newPassword": that.newPassword,
						"oldPassword": that.oldPassword,
					},
					success(res) {
						var data = res.data;
						console.log(data);
						// 弹出提示
						uni.showToast({
							icon: 'success',
							title: data.msg,
						})
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
