<template>
	<view class="page">
<uploading v-if='isuploading'></uploading>
		<view class="box" v-if="searching">
			<!-- 头部 -->

			<view class="top_box" v-if="hidden">
				<!-- 分类 -->
				<view class="top_right" @click="clas">
					<text>分类</text>
					<image src="../../static/images/group.png" mode=""></image>
				</view>
				<!-- 三个小图标 -->
				<view class="top_icon">
					<view>
						<image class="searchimg" src="../../static/images/search.png" mode=""></image>
					</view>
					<view @click="showfile">
						<image class="fileimg" src="../../static/images/new_file.png" mode=""></image>
					</view>
					<view @click="showsort">
						<image class="sortimg" src="../../static/images/sort.png" mode=""></image>
					</view>
				</view>
				<!-- 选择 -->
				<view class="top_select" @click="upLoad">
					<image class="transmission" src="../../static/images/transmission.png" mode=""></image>
					<text>传输</text>
				</view>
			</view>

		</view>
		<!-- 全选 取消 -->
		<view class="all" v-if='all'>
			<text @click="chooesActive">全选</text>
			<text @click="hiddenSelect" v-if="allselect">取消全选</text>
			<text @click="quxiao">取消</text>

		</view>



		<view class="c_box">

			<checkbox-group class="uni-list" @change="checkboxChange">
				<label v-for="(item,index) in list" :key="index">
					<view class="center_box">
						<view class="left_box">
							<image class="middle_img" src="../../static/images/file.png" mode=""></image>
							<view class="text_left">
								<view class="text_title">{{item.name}}</view>
								<view class="text_time">{{item.date}}</view>
							</view>
						</view>
						<view>
							<checkbox class="checkbox" v-if="select" :value='item.name' color="#0188FB">
								<!-- <image class="checkbox_img" src="../../static/images/select.png" mode=""></image> -->
							</checkbox>
							<view class="box_img" v-if="ismore" @click="chooes">
								<image class="more" src="../../static/images/more.png" mode=""></image>
							</view>
						</view>
					</view>


					<view class="middle_line"></view>
				</label>

			</checkbox-group>
		</view>


		<!-- 分类组件 -->
		<view class="box">
			<view class="tip" v-if="issort">
				<view class="b_title">
					<text>文件排序</text>
				</view>
				<view class="item_boxt" @click="cancel()">

					<text class="b_time">按时间排序</text>
					<image class="b_img" src="../../static/images/select.png" />
				</view>
				<view class="lines"></view>
				<view class="item_boxn" @click="cancel()">
					<text class="b_name">按名称排序</text>
					<image class="b_img" src="../../static/images/circular.png" />
				</view>
			</view>
		</view>



		<!-- 新建文件 -->
		<view class="box">
			<view class="tip" v-if="isfile">
				<view class="box_title">
					新建文件夹
				</view>
				<view class="box_center">
					<input type="text" placeholder='请输入文件夹名称' v-model="cFile" />
				</view>
				<view class="box_btn">
					<text class="cancel" @click="file">取消</text>
					<text class="create" @click="createFile">创建</text>
				</view>
			</view>
		</view>
		<!-- 重命名 -->
		<view class="box">
			<view class="tip" v-if="isrename">
				<view class="box_title">
					重命名
				</view>
				<view class="box_center">
					<input type="text" placeholder='请输入新的名称' v-model="name" />
				</view>
				<view class="box_btn">
					<text class="cancel" @click="reNameCencl">取消</text>
					<text class="create" @click="rename">确定</text>
				</view>
			</view>
		</view>


		<!-- 搜索组件 -->

		<view v-if='issearch'>
			<view class="s_contain_box">
				<view class="s_box">
					<image src="../../static/images/search.png" />
					<input type="text" @tap='addToHistory' placeholder='请输入搜索关键词' v-model='inputsearch' focus="true" />
					<image class="del" src="../../static/images/delete.png" />
				</view>

				<view>
					<text class="stext" @click="onCancel">
						取消
					</text>
				</view>
			</view>

			<view class="history">
				<view class="h_title">
					<view class="h_left">
						<view class="h_icon">
							<image src="../../static/images/search.png" mode=""></image>
						</view>
						<text>历史搜索</text>
					</view>
					<view class="s_delete">
						<image src="../../static/images/search_delete.png" mode=""></image>
					</view>
				</view>
			</view>
			<view class="h_line"></view>
			<view class="h_clear">
				<text>清空历史记录</text>
			</view>
		</view>


		<!-- 分类组件 -->
		<cla v-if="isclass"></cla>

		<!-- 上传功能 -->
		<upload v-if='isupload' @getFile='getFile' @hiddenUpLoad='hiddenUpLoad'></upload>
		
		<!-- 上传中的功能 -->
		

		<!-- 下载功能 -->
		<view class="b_box1" v-if="select">
			<view class="list">
				<view class="item1">下载</view>
				<view class="item1">移动</view>
				<!-- <view class="item1" @click="rename">重命名</view> -->
				<view class="item1" @click="popRename">重命名</view>
				<view class="del1" @click="delfile">删除</view>
				<!-- <view class="del1">删除</view> -->
			</view>
		</view>


	
	</view>

</template>

<script>
	
	const pro = uni.getStorageSync('pro')
	const token = uni.getStorageSync('token');

	import cla from '../component/cla'
	import upload from '../component/upload'
	import uploading from '../component/uploading'

	export default {
		data() {
			return {
				issort: false,
				isfile: false,
				searching: true,
				issearch: false,
				isclass: false,
				select: false,
				ismore: true,
				list: [],
				inputsearch: '',
				cFile: '',
				all: false,
				hidden: true,
				allselect: false,
				items: [],
				checked: false,
				arr: [],
				newName: '',
				name: '',
				isrename: false,
				isupload: false,
				isuploading:false



			}
		},
		onLoad() {
			// token.cookie = "login_key=" + token.login_key + ';user_id=' + token.user_id + ';isok=' + isok + '';
			// console.log(token);

			if (token) {
				this.getFile()
			} else {
				// 用户没登录，强制跳转到登录页面
				uni.showModal({
					showCancel: false,
					title: '未登录',
					content: '您未登录，需要登录后才能继续',
					success: (result) => {
						// console.log("confirm = " + result.confirm);
						if (result.confirm) {
							uni.reLaunch({
								url: '../component/login/login'
							})
						}
					}
				})
			}
			// this.getFile()

		},
		methods: {
			showsort() {
				this.issort = true
				this.isfile = false
				this.isclass = false
				this.isfile = false

			},
			showfile() {
				this.isfile = true
				this.issort = false
				this.isclass = false
			},
			// 展示搜索框
			showsearch() {
				this.searching = false
				this.issearch = true
				this.isclass = false
				this.isfile = false
				this.issort = false
			},
			onCancel() {
				this.issearch = false
				this.searching = true
			},
			cancel() {
				this.issort = false;
			},
			file() {
				this.isfile = false
			},
			clas() {
				this.isclass = true
				// this.isclass = false
				this.isfile = false
				this.issort = false
			},
			getFile() {
				uni.request({
					url: 'http://192.168.31.9/api/files/listfile',
					method: 'post',
					dataType: "json",
					data: {
						"path": "/"
					},
					success: res => {
						console.log(res.data.data.list);
						this.list = res.data.data.list

					},
					header: {
						"token": token
						// "Cookie": "user_id=1; login_status=ok; login_key=6755d4bc360597c18784e950350b0c13"

						// "Cookie":"user_id=1; login_status=ok; login_key=6755d4bc360597c18784e950350b0c13"

					}

				});
			},
			// 创建文件夹功能
			createFile() {
				// 如果创建成功 重新获取一次列表 把新创建的文件 添加到第一位 unshift
				uni.request({
					url: 'http://192.168.31.9/api/files/createFolder',
					method: 'POST',
					dataType: "json",
					data: {
						"path": '/' + this.cFile
					},
					success: (res) => {
						// console.log(res.data);
						this.getFile()
						if (res.data.ret == 200) {
							this.isfile = false
							uni.showToast({
								icon: 'none',
								title: '文件夹创建成功'
							})
							this.cFile = ''
						} else {
							uni.showToast({
								icon: 'none',
								title: '文件已存在',

							})
						}
					},
					header: {
						// 'Content-Type': 'application/json',
						"token": token
						// "Cookie": "user_id=1; login_	status=ok; login_key=6755d4bc360597c18784e950350b0c13"
					}
				})

			},
			// 选择功能
			chooes() {
				this.ismore = false;
				this.select = true;
				this.all = true
				this.hidden = false
				this.isfile = false
				this.issort = false
				this.isclass = false
				this.isupload = false

			},
			chooesActive(e) {
				console.log(e);
				this.select = true
				this.select = false

				this.allselect = true

			},
			hiddenSelect() {
				this.select = true

				this.allselect = false

			},
			// 只有当我点击选择的时候 小圆圈才会出来
			// 取消功能
			quxiao() {
				this.ismore = true
				this.all = false
				this.select = false
				this.hidden = true
				this.isrename = false
			},
			// 取消全选
			nochose() {

				this.select = true
			},
			selectOne(item, index) {
				// console.log(item,index);
				// console.log(this.select_active = true);

			},
			checkboxChange(e) {
				var checked = e.target.value
				this.items = checked
				// console.log(this.items);
				var arr = this.items.join(" ").split(".")
				// console.log(arr);

			},

			// 删除功能
			delfile(index) {



				let that = this

				if (this.items.length !== 0) {
					uni.showModal({
						showCancel: true,
						title: '删除',
						content: '是否删除',
						success: (result) => {

							if (result.confirm) {
								// console.log(this.items);

								for (let i in this.items) {
									this.arr.push('/' + this.items[i])
								}
								uni.request({
									url: "http://192.168.31.9/api/files/Delete",
									method: 'post',
									header: {
										"token": token
										// "Cookie": "user_id=1; login_status=ok; login_key=6755d4bc360597c18784e950350b0c13"
									},
									data: {
										"items": this.arr,
										"dirs": this.arr
									},
									success(res) {
										console.log(res.data);
										that.getFile()
									}
								})

								uni.showToast({
									icon: 'none',
									title: '删除成功'
								})
								this.ismore = true
								this.all = false
								this.select = false
								this.hidden = true
							}
						}
					})
				}
			},
			rename() {
				var that = this
				var arr = that.items
				console.log(arr);
				console.log(arr.join("").split(".").slice(-1).join());
				var zss = '.' + arr.join("").split(".").slice(-1).join()
				// console.log(that.name);


				if (that.items == that.name) {
					uni.showToast({
						icon: 'none',
						title: '命名相同',
					})
				} else {
					uni.request({
						url: "http://192.168.31.9/api/files/Rename",
						method: 'post',
						header: {
							"token": token
							// "Cookie": "user_id=1; login_status=ok; login_key=6755d4bc360597c18784e950350b0c13"
						},
						data: {
							"path": '/' + arr,
							"Rename_to": that.name
						},
						success(res) {
							console.log(res);
							// console.log(that.items == that.name);
							if (res.data.ret == 200) {
								// console.log(123);	

								uni.showToast({
									icon: 'none',
									title: '重命名成功',
								})


								that.getFile()
								that.isrename = false
								that.all = false
								that.hidden = true
								that.ismore = true

							} else {
								uni.showToast({
									icon: 'none',
									title: '新名称已存在',
								})
							}
						}
					})
				}

			},
			reNameCencl() {
				this.isrename = false
				this.select = true
				this.items = []
			},
			popRename() {
				var that = this
				if (that.items.length == 1) {
					that.isrename = true
					that.select = false
					// that.items = []

				} else if (that.items.length == 0) {
					uni.showToast({
						icon: 'none',
						title: '请选择文件夹'
					})
				} else {
					uni.showToast({
						icon: 'none',
						title: '只能单个命名',
					})
				}
			},
			// 显示上传
			upLoad() {
				this.isupload = true
			},
			// 隐藏上传功能
			hiddenUpLoad() {
				this.isupload = false
								
			}

		},
		components: {
			cla,
			upload,
			uploading
		}
	}
</script>

<style>
	.page {
		padding-bottom: 1upx;
	}

	/* 头部盒子 */
	.top_box {
		display: flex;
		justify-content: space-around;
		align-items: center;
		/* margin-bottom: 50upx; */



	}

	.box {
		position: fixed;
		width: 100%;
		/* top: 90upx; */
		top: 0;
		background-color: #FFFFFF;


	}


	/* 左边分类 */
	.top_right {
		display: flex;
		align-items: center;
	}

	.left_box {
		display: flex;
	}

	.top_right>image {
		width: 24upx;
		height: 24upx;
	}

	.top_right>text {
		font-size: 30upx;
	}

	/* 搜索 文件 排序 图标 */
	.top_icon {
		display: flex;
		justify-content: space-around;
		align-items: center;
		width: 362upx;
		height: 68upx;
		background-color: #f6f6f6;
		border-radius: 18upx;

	}

	.transmission {
		margin-right: 4upx;
		width: 24upx;
		height: 24upx;
	}

	.searchimg {
		width: 37upx;
		height: 37upx;
	}

	.fileimg {
		width: 44upx;
		height: 34upx;
	}

	.sortimg {
		width: 40upx;
		height: 36upx;
	}

	.top_select {
		display: flex;
		align-items: center;
		font-size: 30upx;
	}

	/* 中间样式 */
	.middle_box {
		margin-top: 50upx;
		display: flex;
		justify-content: space-between;
		align-items: center;

	}

	.checkbox {
		position: relative;
		top: 10upx;
		left: -50upx;
		transform: scale(0.8)
	}

	.middle_img {
		width: 86upx;
		height: 72upx;
		margin-left: 37upx;
	}

	.middle_text {
		margin-left: 40upx;
		display: flex;
	}

	.more {
		width: 32upx;
		height: 8upx;
		margin-right: 80upx;
	}

	.select_kong {
		width: 32upx;
		height: 32upx;
		margin-right: 40upx;
	}

	.active {
		width: 32upx;
		height: 32upx;
		margin-right: 40upx;
	}

	/* 全选 取消 all */
	.all {
		font-size: 30upx;
		margin-left: 36upx;
		display: flex;
		justify-content: space-between;
		position: fixed;
		width: 90%;
		z-index: 99999999999;
		top: 0;
		background-color: #FFFFFF;
	}

	/* 文字 */
	.text_left {
		margin-left: 40upx;
	}

	.text_title {
		font-size: 30upx;
		font-family: NotoSansHans;
		font-weight: 400;
		color: rgba(40, 40, 40, 1);
	}

	.text_time {

		font-size: 22upx;
		font-family: NotoSansHans;
		font-weight: 400;
		color: rgba(188, 188, 188, 1);
	}

	.center_box {

		margin-top: 40upx;
		display: flex;
		justify-content: space-between;
	}

	.c_box {
		/* height: 100upx; */
		/* margin-top: 100upx; */
		/* background-color: #000000; */
		/* 	position: relative;
		top: 50upx;
		bottom: 200upx; */
		margin-top: 100upx;
		margin-bottom: 98upx;
	}

	.middle_line {
		margin-top: 40upx;
		margin-bottom: 40upx;
		margin-left: 20upx;
		width: 90%;
		border: 0.5px solid #ccc;
	}




	/* 搜索 */
	.s_box {
		width: 596upx;
		height: 72upx;
		border-radius: 36upx;
		display: flex;
		align-items: center;
		background-color: #F6F6F6;
	}

	.s_box>image {
		width: 28upx;
		height: 28upx;
		margin-left: 32upx;

	}

	.s_box>input {
		margin-left: 20upx;
	}

	.s_contain_box {
		margin-top: 10upx;
		display: flex;
		margin-left: 30upx;

	}


	.del {
		position: absolute;
		right: 150upx;
		width: 18upx;
		height: 18upx;
	}

	.stext {
		margin-left: 30upx;
		font-size: 32upx;
		font-family: NotoSansHans;
		font-weight: 400;
		color: rgba(1, 136, 251, 1);
	}

	/* histroy */
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
		margin-top: 30upx;
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

	/* 分类 */
	.tip {
		position: fixed;
		bottom: 0;
		width: 750upx;
		height: 430upx;
		background-color: #FFFFFF;
		border-radius: 40upx 40upx 0px 0px;
		border: 1px solid #ccc;
		z-index: 99999;
	}

	.b_title {
		text-align: center;
		margin-top: 54upx;
		font-size: 34upx;
		font-family: NotoSansHans;
		font-weight: 500;
		color: rgba(40, 40, 40, 1);
		line-height: 51upx;
	}

	.b_time {

		margin-left: 60upx;
		font-size: 34upx;
		font-family: NotoSansHans;
		font-weight: 400;
		color: rgba(40, 40, 40, 1);
	}

	.b_name {

		margin-left: 60upx;
		font-size: 34upx;
		font-family: NotoSansHans;
		font-weight: 400;
		color: rgba(40, 40, 40, 1);
	}

	.b_img {
		width: 32upx;
		height: 32upx;
		position: relative;
		right: 60upx;
	}

	.item_boxt {
		margin-top: 70upx;
		/* background-color: #ccc; */
		display: flex;
		justify-content: space-between;
		align-items: center;
	}

	.item_boxn {
		margin-top: 50upx;
		/* background-color: pink; */
		display: flex;
		justify-content: space-between;
		align-items: center;
	}

	.lines {
		width: 86%;
		margin: 50upx auto;
		display: flex;
		border: 0.5px solid #CCCCCC;
	}

	/* 新建文件 */
	.tip {
		position: fixed;
		bottom: 0;
		width: 750upx;
		height: 430upx;
		background-color: #FFFFFF;
		border-radius: 40upx 40upx 0px 0px;
		border: 1px solid #ccc;
		z-index: 999999;
	}

	.box_title {
		margin-left: 48upx;
		margin-top: 54upx;
		font-size: 34upx;
		font-family: NotoSansHans;
		font-weight: 500;
		color: rgba(40, 40, 40, 1);
		line-height: 51upx;
	}

	.box_center {
		margin-top: 52upx;
		width: 654upx;
		height: 108upx;
		background: rgba(238, 238, 238, 1);
		border-radius: 54upx;
		margin-left: 48upx;
		align-items: center;
		display: flex;
	}

	.box_center>input {
		margin-left: 50upx;
		font-size: 32upx;
		font-family: NotoSansHans;
		font-weight: 400;
		color: rgba(173, 173, 173, 1);

	}

	.box_btn {
		margin-top: 60upx;
		display: flex;
		justify-content: space-around;
	}

	.cancel {
		font-size: 36upx;
		font-family: NotoSansHans;
		font-weight: 500;
		color: rgba(102, 102, 102, 1);
	}

	.create {
		font-size: 36upx;
		font-family: NotoSansHans;
		font-weight: 500;
		color: rgba(1, 136, 251, 1);
	}

	.b_box {
		position: fixed;
		bottom: 0;
		border-top: 0.5px solid #ccc;
		background: rgba(255, 255, 255, 0.7);
		width: 750upx;
	}

	.b_box1 {
		position: fixed;
		height: 100upx;
		bottom: 0upx;
		border-top: 0.5px solid #ccc;
		background: rgba(255, 255, 255, 0.7);
		/* background-color: #ccc; */
		width: 750upx;
		z-index: 999999999999;
	}

	.list {
		display: flex;
		height: 100upx;
		/* align-items: center; */
		justify-content: space-around;
		line-height: 100upx;
	}

	.del {
		color: #FFC4C4;
		font-size: 34upx;
		font-family: NotoSansHans;
		font-weight: 400;
	}

	.item {
		font-size: 34upx;
		font-family: NotoSansHans;
		font-weight: 400;
		color: #D1D1D1;
	}

	.del1 {
		color: #FF3B3B;
		font-size: 34upx;
		font-family: NotoSansHans;
		font-weight: 400;
	}

	.item1 {
		font-size: 34upx;
		font-family: NotoSansHans;
		font-weight: 400;
		color: #666666;
	}
</style>
