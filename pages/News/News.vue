<template>
	<view>
		<!-- 新闻模块开始 -->
		<view class="box">
			<scroll-view class="scroll-x" scroll-x="true">
				<block v-for="(item, index) in tabList" :key="index">
					<text class="title" @click="choose(item.id)" v-if="item.id != type">{{item.name}}</text>
					<text class="title itemTabSelect" @click="choose(item.id)" v-else>{{item.name}}</text>
				</block>
			</scroll-view>

			<!-- 内容体 -->
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
		<!-- 新闻模块结束 -->
	</view>
</template>

<script>
	const urlHead = getApp().globalData.urlHead;
	export default {
		data() {
			return {
				tabList: [], //存储新闻分类
				newDataList: [], //存储新闻列表
				type: 0, //新闻分类的id
			}
		},
		onShow() {
			this.getAllNewsTab();
		},
		methods: {
			// 新闻详情跳转
			goNewItem(index) {
				uni.navigateTo({
					url: '../index/xwxq?id=' + index,
				})
			},
			// 获取新闻列表
			getNewList(type) {
				let that = this
				uni.request({
					url: urlHead + "/prod-api/press/press/list?type=" + type,
					method: "GET",
					success(res) {
						var data = res.data
						that.newDataList = data.rows
					}
				})
			},

			// 新闻类别切换
			choose(index) {
				this.type = index
				this.getNewList(this.type);
			},


			// 获取新闻分类
			getAllNewsTab() {
				let that = this;
				uni.request({
					url: urlHead + '/prod-api/press/category/list',
					method: 'GET',
					success(res) {
						var data = res.data;
						that.tabList = data.data;
						that.type = that.tabList[0].id;
						that.getNewList(that.type);
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
	/* 新闻 */
	.box {
		background: #FEFEFE;
		border-radius: 5px;
	}

	.scroll-x {
		display: flex;
		white-space: nowrap;
	}

	.title {
		padding: 0 30rpx;
		font-size: 18px;
	}

	/* 新闻内容 */

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


	/* 标签选中变色 */
	.itemTabSelect {
		color: #007AFF;
		font-size: 20px;
		font-weight: bold;
		border-bottom-width: 4px;
	}
</style>
