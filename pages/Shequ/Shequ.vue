<template>
	<view>
		<!-- 新闻公告开始 -->
		<view class="topBanner">
			<swiper autoplay interval="2500" class="banner">
				<block v-for="(item,index) in bannerList" :key="index">
					<swiper-item>
						<view style="width: 100%;height: 100%;" @click="lookPropertyNotice(index)">
							<image :src="item.image" mode="scaleToFill" class="bannerItem" />
							<view class="propertyNotice">{{item.label}}</view>
						</view>
					</swiper-item>
				</block>
			</swiper>
		</view>
		<!-- 新闻公告结束 -->

		<!-- 服务列表开始 -->
		<view style="display: flex;width: 100%;">
			<block v-for="(item,index) in serviceList" :key="index">
				<view style="width: 25%;text-align: center;" @click="jumpToService(item.url)">
					<image :src="item.image" mode="aspectFit" style="width: 96rpx;height: 96rpx;"></image>
					<view>{{item.label}}</view>
				</view>
			</block>
		</view>
		<!-- 服务列表结束 -->

		<!-- 商业推广开始 -->
		<view>
			<view style="font-size: 48rpx;font-weight: bold;margin: 24rpx;">商业推广</view>
			<block v-for="(item,index) in promoteList" :key="index">
				<view class="promoteItem" @click="jumpToTuiguan(index)">
					<image :src="item.image" style="width: 100%;"></image>
					<text style="font-size: 40rpx;">{{item.title}}</text>
				</view>
			</block>
		</view>
		<!-- 商业推广结束 -->



	</view>
</template>

<script>
	export default {
		data() {
			return {
				// banner通知数据
				bannerList: [{
					image: "/static/h.png",
					label: "物业通知\t2022-01-20\t12:00:00"
				}, {
					image: "/static/hh.png",
					label: "物业通知\t\t2022-01-17\t13:00:00"
				}, {
					image: "/static/huniango.jpg",
					label: "物业通知\t\t2022-01-10\t11:00:00"
				}],

				// 服务列表
				serviceList: [{
					image: "/static/tabBar/huodong1.png",
					label: "物业服务",
					url: "wuye"
				}, {
					image: "/static/tabBar/huodong1.png",
					label: "快件管理",
					url: "kuaijian"
				}, {
					image: "/static/tabBar/huodong1.png",
					label: "友邻社交",
					url: "youlin"
				}, {
					image: "/static/tabBar/huodong1.png",
					label: "车辆管理",
					url: "carManage"
				}],

				// 商业推广
				promoteList: [{
					image: "/static/tg.png",
					title: "潮人说：冲在虎年氛围感第一线！",
					contnet: "了解清楚问题到底是一种怎么样的存在，是解决一切问题的关键。 在这种困难的抉择下，本人思来想去，寝食难安。 卡莱尔说过一句著名的话，过去一切时代的精华尽在书中。这不禁令我深思. 这样看来， 从这个角度来看， 我们都知道，只要有意义，那么就必须慎重考虑。 而这些并不是完全重要，更加重要的问题是， 富勒说过一句著名的话，苦难磨炼一些人，也毁灭另一些人。这不禁令我深思. 我们一般认为，抓住了问题的关键，其他一切则会迎刃而解。"
				}],
			}
		},
		methods: {
			// 公告详情
			lookPropertyNotice(index) {
				uni.navigateTo({
					url: "propertyNotice?index=" + index
				})
			},

			// 服务跳转
			jumpToService(index) {
				uni.navigateTo({
					url: index
				})
			},

			// 商业推广
			jumpToTuiguan(index) {
				uni.navigateTo({
					url: "tuiguang?index=" + index
				})
			}
		}
	}
</script>

<style>
	/* banner样式 */
	.propertyNotice {
		background-color: rgba(0, 0, 0, 0.6);
		position: absolute;
		bottom: 0;
		width: 100%;
		color: #FFFFFF;
		padding-left: 24rpx;
	}

	/* banner样式 */
	.banner {
		margin: 32rpx 48rpx;
		padding: 1rpx;
		border: #000000 solid 1rpx;
		border-radius: 24rpx;
		overflow: hidden;
	}

	/* 服务入口图标样式 */
	.promoteItem {
		width: 85%;
		height: 358rpx;
		display: flex;
		flex-direction: column;
		align-items: center;
		margin: 0 64rpx;
		border: #000000 solid 1rpx;
		margin-bottom: 24rpx;
		border-radius: 36rpx;
		overflow: hidden;
	}


	/* 商业推广样式 */
	.bannerItem {
		width: 100%;
		height: 100%;
		justify-content: center;
		align-items: center;
	}
</style>
