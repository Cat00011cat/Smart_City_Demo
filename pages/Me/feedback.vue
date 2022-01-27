<template>
	<view>
		标题
		<input class="bg" placeholder="请输入标题" v-model="title" />
		内容
		<textarea class="bg" placeholder="请描述您遇到的问题0/150字" maxlength="150" v-model="content"></textarea>
		<button @click="submit()">提交</button>
	</view>
</template>

<script>
	// 温习提示，，，把刚开局的ip拿进来
	const urlHead = getApp().globalData.urlHead;
	export default {
		data() {
			return {
				title: '',
				content: '',
			}
		},
		methods: {
			submit() {
				let that = this;
				console.log(that.title);
				console.log(that.content);

				uni.request({
					url: urlHead + '/prod-api/api/common/feedback',
					method: 'POST',
					header: {
						'Authorization': uni.getStorageSync('Authorization'),
					},
					data: {
						"content": that.content,
						"title": that.title
					},
					// 成功回调
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
		}
	}
</script>

<style>
	.bg {
		padding: 15rpx;
		margin-bottom: 30rpx;
		border-bottom: 1px solid #cacacc;
	}
</style>
