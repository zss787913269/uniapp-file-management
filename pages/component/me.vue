<template>
	<view class="content">
		<view class="top">
			<image class="img" src="../../static/images/pic.png" mode=""></image>
			<text class="tex">刘选鸿</text>
		</view>

		<view class="center">

			<view class="center-list-item">
				<text class="list-text">帮助与反馈</text>
				<image class="icon" src="../../static/images/org1.png" mode=""></image>
			</view>

			<view class="center-list-item1" @click="loginOut">
				<text class="list-text1" >退出账号</text>
			</view>

			<!-- <view class="center-list-item1">
				<navigator url="login/login">
					<text class="list-text1">登录界面</text>
				</navigator>
			</view> -->

		</view>
	</view>
</template>
<script>
	export default {
		data() {
			return {

			}
		},
		onLoad() {

		},
		methods: {
			loginOut() {

				uni.request({
					url: 'http://192.168.31.9/api/user/loginOut',
					success(res) {

						console.log(res.data);
						if (res.data.ret == 200) {

					// 		uni.removeStorage({
					// 			key: 'token',
					// 			success: function(res) {
					// 				
					// 
					// 			}
					// 		});
							uni.reLaunch({
								url: 'login/login'
							})

						}
					}
				})
			}
		}
	}
</script>

<style>
	.top {
		display: flex;
		align-items: center;
		margin-top: 100upx;
	}

	.img {
		width: 130upx;
		height: 130upx;
		margin-left: 39upx;

	}

	.tex {
		font-size: 48upx;
		color: #282828;
		margin-left: 30upx;

	}

	.center-list-item {
		display: flex;
		align-items: center;
		justify-content: space-between;
		margin-top: 159upx;

	}

	.list-text {
		font-size: 36upx;
		font-family: NotoSansHans;
		font-weight: 500;
		color: rgba(40, 40, 40, 1);
		margin-left: 40upx;


	}

	.center-list-item1 {
		font-size: 36upx;
		font-family: NotoSansHans;
		font-weight: 500;
		color: rgba(40, 40, 40, 1);
		margin-left: 40upx;
		margin-top: 100upx;
	}

	.icon {
		width: 40upx;
		height: 40upx;
		margin-right: 20upx;
	}
</style>
