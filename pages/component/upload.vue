<template>
	<view class="box">
		<!-- <uploading></uploading> -->
		<view class="tip">
			<view class="box_title">
				<view>
					选择上传文件的类型
				</view>

			</view>

			<view class="box_btn">
				<view class="img_box" @click="photo">
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
			<view class="box_btn1">
				<view class="img_box">
					<image src="../../static/images/voice.png" mode=""></image>
					<view class="text">语音速记</view>
				</view>
				<view class="img_box scanning">
					<image src="../../static/images/scanning.png" mode=""></image>
					<view class="text">智能扫描</view>
				</view>
				<view class="img_box">
					<image mode=""></image>
					<view class="text"> </view>
				</view>
				<view class="img_box scanning" @click="quxiao">
					<image src="../../static/images/delete.png"></image>
					<view class="text">取消传输</view>
				</view>
			</view>

		</view>
	</view>
</template>

<script>
	// import cla from '../component/cla'
	// import uploading from '/uploading'
	const token = uni.getStorageSync('token');
	export default {
		data() {
			return {
				// isshow: true
				isuploading: false,
				list: []
			}
		},
		components: {

		},
		methods: {

			photo() {
				var that = this

				uni.chooseImage({
					success: (res) => {
						console.log(res.tempFilePaths);

						for (let i in res.tempFilePaths) {
							console.log(res.tempFilePaths[i]);

							const tempFilePaths = res.tempFilePaths[i]
							var newDay = new Date()

							var name = Date.parse(newDay)
							var ran = Math.ceil(Math.random() * 100);
							const uploadTask = uni.uploadFile({
								url: 'http://192.168.31.9/api/upload',
								filePath: tempFilePaths,
								name: 'file',
								formData: {
									"name": 'img' + name + ran,
									"path": "/",
									"file": tempFilePaths,
								},
								fileType: 'image',
								success: (result) => {
								
									if (result.statusCode == 200) {

										this.quxiao()
										this.isshow = false
										uni.showToast({
											icon: 'none',
											title: '上传成功',
										})
									} else {
										uni.showToast({
											icon: 'none',
											title: '上传失败',
										})
									}
								},
								header: {
									"token": token
								},

							});
							uploadTask.onProgressUpdate(function(res) {
								// console.log(res);
								var progress = res.progress
								uni.setStorage({
									key: "pro",
									data: progress
								})
							})
						}
					},
				});

			},
			quxiao() {
				this.$emit("getFile")
				this.$emit("hiddenUpLoad")
			},
			showLoading() {
				this.isuploading = true
			}
		},

	}
</script>

<style scoped>
	.tip {
		position: fixed;
		bottom: 0;
		width: 750upx;
		height: 491upx;
		background-color: #FFFFFF;
		border-radius: 40upx 40upx 0px 0px;
		border: 1px solid #ccc;
	}

	.box_title {
		margin-top: 50upx;
		/* margin-left: -100upx */
		text-align: center;
		font-size: 36upx;
		font-family: NotoSansHans;
		font-weight: 500;
		color: rgba(173, 173, 173, 1);
	}

	.box_btn {
		margin-top: 60upx;
		display: flex;
		justify-content: space-around;
		/* flex-wrap: wrap; */
	}

	.box_btn1 {
		margin-top: 40upx;
		display: flex;
		justify-content: space-around;
		/* flex-wrap: wrap; */
		/* margin-left: 58upx; */
		/* margin-bottom: 85upx; */
	}

	.img_box image {
		width: 88upx;
		height: 88upx;
	}

	.text {
		font-size: 22upx;
		font-weight: 400;
		color: rgba(153, 153, 153, 1);
	}

	.img_box {
		text-align: center;
	}

	.scanning {
		margin-left: -2upx;
	}
</style>
