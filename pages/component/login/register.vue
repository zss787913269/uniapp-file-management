<template>
	<view class="login_box">
		<view class="count">
			<text>账号：</text>
			<input type="text" placeholder="请使用邮箱注册账号" v-model="user" maxlength="16">
		</view>
		<view class="psw">
			<text>密码：</text>
			<input type="password" placeholder="请输入密码" v-model="psw" maxlength="16">
		</view>
		<view class="repsw">
			<text>密码：</text>
			<input type="password" placeholder="再次输入密码" v-model="repsw" maxlength="16">
		</view>
		<button type="primary" @click="login">注册</button>
		<view class="text_box">
			<view>
				<text>找回密码</text>|
			</view>
			<navigator url="login">
				<text>立即登录</text>
			</navigator>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				user: '',
				psw: '',
				repsw: ''
			}
		},
		onLoad() {

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
				if (this.psw.length <= 3) {
					uni.showToast({
						icon: 'none',
						title: '密码不能为空:长度4-16位'
					})
					return
				}
				if (this.repsw.length <= 3) {
					uni.showToast({
						icon: 'none',
						title: '密码不能为空:长度4-16位'
					})
					return
				}

				if (this.psw !== this.repsw) {
					uni.showToast({
						icon: 'none',
						title: '两次输入的密码不一样'
					})
					return
				} else {
					uni.request({
						url: 'http://192.168.31.9/api/user/register',
						data: {
							username: this.user,
							pwd: this.psw,
							repwd: this.repsw
						},
						method:'POST',
						success: (res) => {
							if (this.user !== '') {
								var reg = /^[A-Za-z0-9\u4e00-\u9fa5]+@[a-zA-Z0-9_-]+(\.[a-zA-Z0-9_-]+)+$/;
								if (!reg.test(this.user)) {
									uni.showToast({
										icon: 'none',
										title: '请输入有效的邮件'
									})
								}
							}
							console.log(res)

							if (res.data.ret == 200) {
								uni.showToast({
									icon: 'none',
									title: '注册成功'
								})
							
							} else {
								uni.showToast({
									icon: 'none',
									title: '邮箱被注册'
								})
							}

						}
					})
				}
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
	.psw,
	.repsw {
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

	.repsw>text {
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
