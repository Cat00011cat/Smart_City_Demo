<template>
	<view>
		<view class="iconBox" @click="d()">
			<block v-for="(item,index) in serviceList" :key="index">
				<view class="HomeIcons-top-item" @click="toService(index)">
					<!--在这里进行页面跳转逻辑编写 -->
					<image :src="getImgUrl(item.imgUrl)" class="HomeIcons-top-item-icon"></image>
					<view class="HomeIcons-top-item-icon-desc">{{item.serviceName}}</view>
				</view>
			</block>
		</view>
	</view>
</template>

<script>
	const urlHead = getApp().globalData.urlHead;
	export default {
		data() {
			return {
				serviceList: [], //存储服务
			}
		},
		onLoad() {
			this.getServiceList();
		},
		methods: {
			d() {
				uni.navigateTo({
					url: 'park/index.vue'
				})
			},
			//服务跳转
			toService(index) {
				console.log(index)
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
						that.serviceList = data.rows;
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
	/* 服务板块 */
	/* 背景 */
	.iconBox {
		margin: 15rpx;
		height: 710rpx;
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
</style>
