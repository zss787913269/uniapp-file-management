<template>
	<view class="box"> 
		<view class="tip" v-if="isshow">
			<view class="box_title">
				新建文件夹
			</view>
			<view class="box_center">
				<input type="text"  placeholder='请输入文件夹名称' />
			</view>
			<view class="box_btn">
				<text class="cancel" @click="cancel">取消</text>
				<text class="create">创建</text>
			</view>
		</view>
	</view>
</template>

<script>
	export default{
		data(){
			return{
				isshow:true
			}
		},
		methods:{
			cancel(){
				this.isshow = false;
			}
		}
	}
</script>

<style scoped>

	.tip{
		position: fixed;
		bottom: 0;
		width: 750upx;
		height: 430upx;
		background-color: #FFFFFF;
		border-radius:40upx 40upx 0px 0px;
		border: 1px solid #ccc;
		z-index: 999999;
	}
	.box_title{
		margin-left: 48upx;
		margin-top: 54upx;
		font-size:34upx;
		font-family:NotoSansHans;
		font-weight:500;
		color:rgba(40,40,40,1);
		line-height:51upx;
	}
	.box_center{
		margin-top: 52upx;
		width:654upx;
		height:108upx;
		background:rgba(238,238,238,1);
		border-radius:54upx;
		margin-left: 48upx;
		align-items: center;
		display: flex;
	}
	.box_center>input{
		margin-left: 50upx;
		font-size:32upx;
		font-family:NotoSansHans;
		font-weight:400;
		color:rgba(173,173,173,1);
	
	}
	.box_btn{
		margin-top: 60upx;
		display: flex;
		justify-content:space-around;
	}
	.cancel{
		font-size:36upx;
		font-family:NotoSansHans;
		font-weight:500;
		color:rgba(102,102,102,1);
	}
	.create{
		font-size:36upx;
		font-family:NotoSansHans;
		font-weight:500;
		color:rgba(1,136,251,1);
	}
</style>
