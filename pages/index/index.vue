<template>
	<view>

		<!-- 搜索框开始 -->
		<view class="searchBox">
			<view class="searchBg">
				<input placeholder="假装有搜索" v-model="searchValue" confirm-type="search"
					@confirm="doSearch(searchValue)" />
			</view>
		</view>
		<!-- 搜索框结束 -->


		<!-- 轮播图开始 -->
		<view>
			<swiper circular="true" indicator-dots="true" class="swiperBox" autoplay="true">
				<block v-for="(item,index) in bannerList" :key="index">
					<swiper-item>
						<image :src="getImgUrl(item.advImg)" />
					</swiper-item>
				</block>
			</swiper>
		</view>
		<!-- 轮播图结束 -->


		<!-- 全部服务开始 -->
		<view class="iconBox">
			<block v-for="(item,index) in serviceList" :key="index">
				<view class="HomeIcons-top-item" @click="toService(index)">
					<!--在这里进行页面跳转逻辑编写 -->
					<image :src="getImgUrl(item.imgUrl)" class="HomeIcons-top-item-icon"></image>
					<view class="HomeIcons-top-item-icon-desc">{{item.serviceName}}</view>
				</view>
			</block>
			<!-- 更多服务 -->
			<view class="HomeIcons-top-item" @click="toAllservice()">
				<image src="../../static/tabBar/more.png" class="HomeIcons-top-item-icon"></image>
				<view class="HomeIcons-top-item-icon-desc">更多服务</view>
			</view>
		</view>
		<!-- 全部服务结束 -->


		<!-- 热门主题模块开始 -->
		<view>
			<view class="themeBoxL" @click="goTheme(0)">
				<image src="http://wx3.sinaimg.cn/bmiddle/006CXrEjly1gr48ftok6mj30n00mutc4.jpg"
					style="width: 175px;height: 70px;"></image>
				<view>主题主题主题主题主....</view>
			</view>
			<view class="themeBoxR" @click="goTheme(1)">
				<image src="http://wx1.sinaimg.cn/bmiddle/ceeb653ely1g5q9mk5jjqj206o06o74a.jpg"
					style="width: 175px;height: 70px;"></image>
				<view>主题主题主题主题主.....</view>
			</view>
		</view>
		<!-- 热门主题模块结束 -->


		<!-- 新闻模块开始 -->
		<view>
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
		</view>
		<!-- 新闻模块结束 -->


	</view>
</template>

<script>
	// 在网络请求之前一定要先常量声明
	// const 声明一个只读的常量，一旦声明，常量的值就不能改变。
	const urlHead = getApp().globalData.urlHead;

	export default {
		data() {
			return {
				bannerList: [], //存储banner
				serviceList: [], //存储服务
				tabList: [], //存储新闻分类
				newDataList: [], //存储新闻列表
				type: 0, //新闻分类的id
				searchValue:''	//存储获取用户输入的搜索内容

			}
		},
		onLoad() {
			this.getBannerList();
			this.getServiceList();
		},
		onShow() {
			this.getAllNewsTab();
		},
		methods: {
			// 回车获取到用户输入的值并跳转
			doSearch(searchValue) {
				let that = this;
				uni.navigateTo({
					url: 'searchValue?value=' + that.searchValue,
				})
			},

			// 新闻详情跳转
			goNewItem(index) {
				uni.navigateTo({
					url: 'xwxq?id=' + index,
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



			//热门主题跳转
			goTheme(e) {
				// console.log(e)
				let pageUrl;
				switch (e) {
					case 0:
						pageUrl = 'park/index';
						break;
					case 1:
						pageUrl = 'metro_query/index';
						break;
					default:
						pageUrl = 'index'
				}
				uni.navigateTo({
					url: pageUrl,
				})
			},


			//更多服务跳转
			toAllservice() {
				uni.switchTab({
					url: '../allService/allService'
				})
			},

			//服务跳转
			toService(index) {
				var ulink = this.serviceList[index].link;
				console.log(ulink);
				uni.navigateTo({
					url: ulink,
				})
			},

			//获取全部服务
			getServiceList() {
				let that = this;
				uni.request({
					url: urlHead + "/prod-api/api/service/list",
					method: "GET",
					success(res) {
						var data = res.data;
						that.serviceList = data.rows.length > 9 ? data.rows.slice(0, 9) : data.rows;
					}
				})
			},



			// 获取轮播图
			getBannerList() {
				// 此处使用that是因为在uni.request中，this不是指向这里的this
				// let 声明的变量只在 let 命令所在的代码块内有效。
				let that = this;
				uni.request({
					url: urlHead + "/prod-api/api/rotation/list?type=2",
					method: "GET",
					success(res) {
						var data = res.data;
						that.bannerList = data.rows;
					}
				})
			},
			// 返回
			getImgUrl(item) {
				return urlHead + item;
			}
		}
	}
</script>

<style>
	.searchBox {
		padding: 15rpx;
	}

	.searchBg {
		background-color: #FFFFFF;
		border-radius: 15rpx;
	}

	/* 轮播图样式 */
	.swiperBox {
		margin: 15rpx;
		height: 151px;
		background-color: #FFFFFF;
		border-radius: 20rpx;
	}

	image {
		width: 710rpx;
		height: 151px;
		border-radius: 10px;
	}

	/* 服务板块 */
	/* 背景 */
	.iconBox {
		margin: 15rpx;
		height: 450rpx;
		background-color: #FEFEFE;
		border-radius: 15rpx;
	}

	.HomeIcons-top-item {
		/* 控制图标排列 */
		/*  向左浮动  目的 图标一行显示*/
		float: left;
		margin: 10px 0px 0px 0px;
		width: 20%;
	}

	/* 图标大小 */
	.HomeIcons-top-item-icon {
		width: 50px;
		height: 50px;
		margin: 0 auto;
		padding: 10px;
	}

	/* 图标名称 */
	.HomeIcons-top-item-icon-desc {
		text-align: center;
	}


	/* 主题 */
	.themeBoxL {
		float: left;
		background-color: #ffffff;
		background-size: 195rpx;
		width: 350rpx;
		height: 200rpx;
		margin-left: 20rpx;
		border-radius: 20px;
	}

	.themeBoxR {
		float: left;
		background-color: #ffffff;
		background-size: 195rpx;
		width: 350rpx;
		height: 200rpx;
		left: 15rpx;
		margin-left: 10rpx;
		border-radius: 20px;
	}


	/* 新闻 */
	.box {
		margin: 230rpx 15rpx 15rpx 15rpx;
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
