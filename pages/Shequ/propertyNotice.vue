<template>
	<view>
		<view style="margin: 12rpx 36rpx;font-size: 36rpx;font-weight: bold;">{{bannerList[index].label}}</view>
		<image :src="bannerList[index].image" mode="scaleToFill" style="width: 90%;margin: 24rpx 36rpx;"></image>
		<view style="padding:36rpx;">
			<rich-text :nodes="bannerList[index].content"></rich-text>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				index: 0,
				bannerList: [{
					image: "/static/h.png",
					label: "物业通知\t2022-01-20\t12:00:00",
					content: "<p>1、振奋精神，迎接挑战，开创未来，再创佳绩！ </p>"
				}, {
					image: "/static/hh.png",
					label: "物业通知\t\t2022-01-20\t13:00:00",
					content: "<div>总有一种守护，始终在身边总有一种力量，催人奋进。心有所信，方能行远。每一个平凡的你我，致敬每个了不起的人。#2022跨年 #致青春拼搏奋斗路上的自己</div>"
				}, {
					image: "/static/huniango.jpg",
					label: "物业通知\t\t2022-01-20\t10:00:00",
					content: "<div>&emsp;若结局非你所愿，就在尘埃前奋力一搏“不要丧啦，说不定你也是别人心中偷偷藏起来的光”“与其因为找不到想做的事而焦躁不已，不如开始做具体的事”</div>"
				}]
			}
		},
		onLoad(option) {
			if (option.index != null && option.index != "") {
				this.index = option.index
				console.log(this.index);
			}
		},
		methods: {

		}
	}
</script>

<style>

</style>
