<template>
	<view class="ubox">
		<view class="uploading" @click="show">
			<image src="../../static/images/sc.png" mode=""></image>
			<text>上传中...</text>
		</view>
		<view class="uploading1" v-if="isuploading1" @click="show1">

			<text>1个文件上传成功</text>
		</view>
		<view class="uploading2" v-if="isuploading2" @click="show2">

			<text>1个文件上传成功，2个文件上传失败</text>
		</view>
	</view>
</template>

<script>
	const pro = uni.getStorageSync('pro')
	export default {
		data() {
			return {
				isuploading:true,
				isuploading1:false,
				isuploading2:false
			}
		},
		onLoad() {
			
			console.log(pro);
		
		},
		methods: {
			show() {
				// 如果进度为100 则显示上传成功  1一个文件上传成功
				this.isuploading = false
				this.isuploading1 = true		
				this.isuploading2 = false
			},
			show1(){
				this.isuploading = false
				this.isuploading1 = false
				this.isuploading2 = true
			},
			show2(){
				this.isuploading = true
				this.isuploading1 = false
				this.isuploading2 = false
			}
			
		},
	}
</script>

<style>
	
	.uploading {
		margin-top: 100upx;
		width: 750upx;
		height: 70upx;
		background-color: #0188FB;
		display: flex;
		justify-content: center;
		align-items: center;

	}

	.uploading>image {
		width: 32upx;
		height: 32upx;
	}

	.uploading>text {
		font-size: 30upx;
		font-family: PingFangSC;
		font-weight: 500;
		color: rgba(255, 255, 255, 1);
	}

	.uploading1 {
		/* margin-top: 20upx; */
		width: 750upx;
		height: 78upx;
		background-color: #0188FB;
		display: flex;
		justify-content: center;
		align-items: center;
	}

	.uploading1>text {
		font-size: 30upx;
		font-family: PingFangSC;
		font-weight: 500;
		color: rgba(255, 255, 255, 1);
	}

	.uploading2 {
		/* margin-top: 20upx; */
		width: 750upx;
		height: 78upx;
		background-color: #F47474;
		display: flex;
		justify-content: center;
		align-items: center;

	}

	.uploading2>text {
		font-size: 30upx;
		font-family: PingFangSC;
		font-weight: 500;
		color: rgba(255, 255, 255, 1);
	}
</style>
