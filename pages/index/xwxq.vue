<template>
	<view>
		<view class="bg">
			<view class="titlea">{{newData.title}}</view>
			<view>
				<text class="time">{{newData.publishDate}}</text>
				<button size="mini" @click="go()" style="left: 15rpx;">去评论</button>
			</view>
			<image :src="getImg(newData.cover)" class="img"></image>
			<rich-text :nodes="newData.content" class="desc" />
		</view>
		<!-- 发表留言 -->
		<textarea placeholder="请输入评论" style="margin: 15rpx; background-color: #FFFFFF; width: 720rpx;border-radius: 15rpx;"></textarea>
		<button>评论</button>
		<!-- 评论列表 -->
		<view>评论条数{{}}</view>
		<view style="margin: 15rpx; background-color: #FFFFFF; width: 720rpx;height: 50rpx;border-radius: 10rpx;">测试评论</view>
		<view style="margin: 15rpx; background-color: #FFFFFF; width: 720rpx;height: 50rpx;border-radius: 10rpx;">测试1111111</view>
		<view style="margin: 15rpx; background-color: #FFFFFF; width: 720rpx;border-radius: 10rpx;">测试2222222222222222222222221111111111111111111111笑死我了</view>
	</view>
</template>

<script>
	const urlHead = getApp().globalData.urlHead;
	export default {
		data() {
			return {
				id: 29,
				newData: [],
			}
		},
		onLoad(e) {
			if (e.id != null) {
				this.id = e.id;
			}
			this.getnewItem()
			this.getNewComment()
		},
		methods: {
			go(){
				uni.pageScrollTo({
					scrollTop:11000,
					duration:300,
				})
			},
			getImg(index) {
				return urlHead + index
			},
			getnewItem() {
				let that = this
				uni.request({
					url: urlHead + "/prod-api/press/press/" + that.id,
					method: "GET",
					success(res) {
						that.newData = res.data.data
					}
				})
			}

		}
	}
</script>

<style>
	/* 新闻内容 */

	/* 模块背景用这个 fefefe   ios简约标准*/
	.bg {
		border-radius: 5px;
		background: #fefefe;
		padding: 10px;
	}

	.titlea {
		font-size: 18px;
		font-weight: bold;
		line-height: 30px;
	}

	.img {
		margin-top: 5px;
		width: 100%;
		height: 150px;
		border-radius: 5px;
	}

	.desc {
		margin-top: 5px;
		font-size: 16px;
		line-height: 30px;
	}

	.time {
		margin-top: 5px;
		font-size: 14px;
		color: #a6a6a6;
	}
</style>
