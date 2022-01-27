<template>
	<view>
		<view v-if="newDataList==0">抱歉哦，没有搜索到“{{searchValue}}”相关的新闻</view>
		<block v-for="(item, index) in newDataList" :key="item.id">
			<view class="bg" @click="goNewItem(item.id)">
				<!-- .slice(0,20)用于指定显示长度 -->
				<view class="titlea">{{item.title}}
					<image :src="getImgUrl(item.cover)" class="img"></image>
				</view>
				<view>
					<rich-text class="desc"
						:nodes="item.content.length > 40 ? item.content.slice(0,40)+'...' : item.content">
					</rich-text>
				</view>
				<view>
					<text class="time">评论总数：{{item.commentNum>0 ? item.commentNum : 0}}</text>
				</view>
				<view>
					<text class="time">发布时间：{{item.publishDate}}</text>
				</view>
			</view>
		</block>
	</view>
</template>

<script>
	// ip拿进来
	const urlHead = getApp().globalData.urlHead;
	export default {
		data() {
			return {
				searchValue: '',
				newDataList: [],
			}
		},
		onLoad(e) {
			var that = this;
			// // e是传进来的参数
			that.searchValue = e.value;
			that.tosearch();
			console.log(that.searchValue);
		},
		methods: {


			// 新闻详情跳转
			goNewItem(index) {
				uni.navigateTo({
					url: 'xwxq?id=' + index,
				})
			},

			// 请求服务端数据
			tosearch() {
				let that = this;
				// 这里要注意的是searchValue不是参数。。。。是返回值。。淦
				// console.log(that.searchValue);
				uni.request({
					url: urlHead + '/prod-api/press/press/list?title=' + that.searchValue,
					method: 'GET',
					success: (res) => {
						var data = res.data
						that.newDataList = data.rows
					}
				})
			},
			getImgUrl(item) {
				return urlHead + item;
			}
		}
	}
</script>

<style>
	.bg {
		margin: 15rpx;
		border-radius: 5px;
		background: #FFFFFF;
		padding: 10px;
	}

	.titlea {
		font-size: 18px;
		font-weight: bold;
		/* 设置行距 */
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

		/*  */
		overflow: hidden;
		word-break: break-all;
		text-overflow: ellipsis;
		display: -webkit-box;
		-webkit-box-orient: vertical;
		-webkit-line-clamp: 2;
	}

	.time {
		margin-top: 5px;
		font-size: 14px;
		color: #a6a6a6;
	}
</style>
