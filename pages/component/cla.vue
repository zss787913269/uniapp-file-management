<template>
	<view class="box">
		<view class="tip" v-if="isshow" @click="cancel">
			<view class="box_title">
				文件分类
			</view>

			<view class="box_btn">
				<view class="img_box">
					<image src="../../static/images/picture.png" mode=""></image>
					<view class="text">图片</view>
				</view>
			<view class="img_box">
					<image src="../../static/images/video.png" mode=""></image>
					<view class="text">视频</view>
				</view>
			<view class="img_box">
					<image src="../../static/images/audio.png" mode=""></image>
					<view class="text">音频</view>
				</view>
			<view class="img_box">
					<image src="../../static/images/doc.png" mode=""></image>
					<view class="text">文档</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				isshow: true
			}
		},
		methods: {
			cancel() {
				this.isshow = false;
			}
		}
	}
</script>

<style scoped>
	.tip {
		position: fixed;
		bottom: 0;
		width: 750upx;
		height: 339upx;
		background-color: #FFFFFF;
		border-radius: 40upx 40upx 0px 0px;
		border: 1px solid black;
		z-index: 99999999;
	}

	.box_title {
		margin-top: 50upx;
		margin-left: -77upx;
		text-align: center;
		font-size: 36upx;
		font-family: NotoSansHans;
		font-weight: 500;
		color: rgba(40, 40, 40, 1);
	}

	.box_btn {
		margin-top: 60upx;
		display: flex;
		justify-content: space-around;
	
	}

	.box_btn image {
		width: 88upx;
		height: 88upx;
	}

	.text {
		font-size: 22upx;
		font-weight: 400;
		color: rgba(153, 153, 153, 1);
	}
	.img_box{
		text-align: center;
	}
</style>
