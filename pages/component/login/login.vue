<template>
	<view class="login_box">
		<view class="count">
			<text>账号：</text>
			<input type="text" placeholder="请输入用户名" v-model="user" maxlength="20">
		</view>
		<view class="psw">
			<text>密码：</text>
			<input type="password" placeholder="请输入密码" v-model="psw" maxlength="20">
		</view>
		<button type="primary" @click="login">登录</button>
		<view class="text_box">
			<view>
				<text>找回密码</text>|
			</view>
			<navigator url="register">
				<text>立即注册</text>
			</navigator>
		</view>
	</view>
</template>

<script>
	const token = uni.getStorageSync('token');
	export default {
		data() {
			return {
				user: 'admin@gmail.com',
				psw: '123456',
			}
		},
		onLoad() {
		// console.log(token);
		},
		methods: {
			login() {
				if (this.user.length <= 0) {
					uni.showToast({
						icon: 'none',
						title: '请输入用户名'
					})
					return
				}
				if (this.psw.length <= 0) {
					uni.showToast({
						icon: 'none',
						title: '请输入密码'
					})
					return
				}
						var login_status = 'ok'
				uni.request({
				
					url: 'http://192.168.31.9/api/user/login',
					data: {
						username: this.user,
						pwd: this.psw
					},
					header:{
							
							// "Cookie":"user_id=token.user_id; login_status=isok; login_key=token.login_key",
							// "Cookie":"user_id=1; login_status=ok; login_key=6755d4bc360597c18784e950350b0c13"
							"token":uni.getStorageSync('token')
						},
					method:'POST',
					success: (res) => {
					
						// console.log(res);
						if (res.data.ret == 200) {

							uni.reLaunch({
								url: '/pages/index/index'
							})
						} else {
							uni.showToast({
								icon: 'none',
								title: '账号或密码错误'
							})
						}
						uni.setStorage({
							key: "token", //将用户信息储存到本地缓存中，重要的uuid及token
							data: res.data.data.token,
							success: function(res) {
								// console.log(res);
							}
						});
							

					},
				})
			}
		}
	}
</script>

<style>
	.login_box {

		display: flex;
		flex-direction: column;
		align-items: center;
	}

	.count,
	.psw {
		border: 1px solid #CCCCCC;
		/* line-height: 100upx; */
		border-radius: 250upx;
		display: flex;
		width: 85%;
		padding: 20upx;
		margin: 20upx;
		/* height: 100upx; */

	}

	.psw>text {
		margin-left: 20upx;
		color: #CCCCCC;
		font-size: 32upx;
	}

	.count>text {
		margin-left: 20upx;
		font-size: 32upx;
		color: #CCCCCC;
	}

	.text_box {
		position: fixed;
		bottom: 20upx;
		display: flex;

	}

	.text_box text {
		color: #2C405A;
		margin: 10upx;
	}

	button {
		border-radius: 30upx;
		margin-top: 50upx;
		width: 85%;
	}
</style>
