<template>
	<view>
		<view class="s_contain_box">
			<view class="s_box">
				<view class="box_left">
					<image src="../../static/images/search.png" />
					<input type="text" placeholder='请输入搜索关键词' maxlength='20' v-model="message" @confirm="searchNow()" />
				</view>
				<view>
					<image class="del" @click="del" src="../../static/images/delete.png" />
				</view>
			</view>

			<view>
				<text>
					取消
				</text>
			</view>
		</view>
		<view class="list_box" v-for="item in fileList" :key='item.id'>
			<text>{{item.name}}</text>
		</view>
		<view class="history">
			<view class="h_title" v-for="(i,index) in historyList" :key='index'>
				<view class="h_left">
					<view class="h_icon">
						<image src="../../static/images/search.png" mode=""></image>
					</view>
					<text>{{i}}</text>
				</view>
				<view class="s_delete" @click="clearOne(i)">
					<image src="../../static/images/search_delete.png" mode=""></image>
				</view>
			</view>
			<view class="h_line"></view>
			<view class="h_clear">
				<text @click="clearAll">清空历史记录</text>
			</view>
		</view>
		<!-- <view  >
			<text>{{i}}</text>
		</view> -->

	</view>
</template>

<script>
	export default {
		data() {
			return {
				message: '',
				fileList: '',
				historyShow: true,
				// historyList 用来接收输入框的历史数据
				historyList: []

			}
		},
		onLoad() {
			// 页面一进去 就获取本地储存数据
			// uni.getStorage({
			// 	key: 'hList',
			// 	// success: function(res) {
			// 	// 	// this.historyList = res.data
			// 	// 	
			// 	// }
			// });
		},
		watch: {
			message(newVal) {
				console.log(newVal);
				if (newVal !== '') {
					this.historyShow = false
					uni.request({
						url: 'http://192.168.31.9/api/files/Searchfile',
						method: 'POST',
						dataType: "json",
						data: {
							"path": this.message
						},
						success: (res) => {
							this.fileList = res.data.result
						},
						header: {
							"Cookie": "user_id=1; login_status=ok; login_key=6755d4bc360597c18784e950350b0c13"
						},
					})
				} else {
					this.fileList = ''
					this.historyShow = true
				}
			}
		},
		methods: {
			del() {
				this.message = ''
			},
			searchNow() {

				let arr = this.historyList
				arr.unshift(this.message)
				this.historyList = arr
				console.log(this.historyList);

				uni.getStorage({
					key: 'hList',
					success: function(res) {
						// 						console.log(arr.indexOf(this.message));

						if (arr.length > 4) {
							arr.pop()
						}
					}
				});
				// 先把值存到本地
				// 再次输入的时候 判断是否跟本地的值一样 如果不一样 就添加 如果一样就不添加
				// 当记录大于五条的时候 从前面删除
				uni.setStorage({
					key: 'hList',
					data: arr,
				});
				// this.message =''
			},
			clearAll() {
				// this.message = ''
				if (this.historyList.length > 0) {
					uni.showModal({
						showCancel: true,
						title: '删除全部',
						content: '确定清空历史记录吗',
						success: (result) => {
							// console.log("confirm = " + result.confirm);
							if (result.confirm) {
								this.historyList = []
								this.message = ''
								uni.removeStorage({
									key: 'hList',
								});
							}
						}
					})
				}

			},
			clearOne(e) {
				console.log(e);
				uni.getStorage({
					key: 'hList',
					success: function(res) {
						// this.historyList = res.data
						// console.log(res.data);
						// // this.historyList.remove(e)
						// console.log(this.historyList);

					}
				});
			}

		}
	}
</script>

<style scoped>
	page {
		background: rgba(255, 255, 255, 1);

	}

	.s_box {
		width: 596upx;
		height: 72upx;
		border-radius: 36upx;
		display: flex;
		align-items: center;
		background-color: #F6F6F6;
		justify-content: space-between;
	}

	.box_left>image {
		width: 30upx;
		height: 30upx;
		margin-left: 32upx;

	}

	.s_box input {
		margin-left: 20upx;
	}

	.s_contain_box {
		margin-top: 10upx;
		display: flex;
		margin-left: 30upx;

	}

	.del {
		margin-right: 10upx;
		width: 30upx;
		height: 30upx;
	}

	text {
		margin-left: 30upx;
		font-size: 32upx;
		font-family: NotoSansHans;
		font-weight: 400;
		color: rgba(1, 136, 251, 1);
	}

	.list_box {
		display: flex;
		flex-wrap: wrap;
		margin-top: 20upx;
		flex-direction: column;
	}

	.list_box text {
		/* margin-top: 20upx; */
		/* width: 100upx;; */
		/* height: 50upx; */
		/* background-color: #CCCCCC; */

	}

	.box_left {
		display: flex;
		align-items: center;
	}

	.h_title {
		display: flex;
		margin-top: 30upx;
		align-items: center;
		justify-content: space-between;
	}

	.h_icon>image {
		margin-left: 30upx;
		width: 28upx;
		height: 28upx;
	}

	.h_title text {
		margin-left: 30upx;
		font-size: 28upx;
		color: #282828;

	}

	.h_line {
		width: 80%;
		margin-top: 20upx;
		/* text-align: center; */
		margin: 30upx auto;
		border: 0.5px solid #ccc;
	}

	.h_clear {
		text-align: center;
		font-size: 28upx;
		color: #999999;
	}

	.s_delete image {
		width: 20upx;
		height: 20upx;
		margin-right: 52upx;
	}

	.h_left {
		display: flex;
		align-items: center;
	}
</style>
