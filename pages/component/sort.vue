<template>
	<view class="box"> 
		<view class="tip" v-if="isshow">
			<view class="b_title">
				<text>文件排序</text>
			</view>
			<view class="item_boxt" @click="cancel()">
		
				<text class="b_time">按时间排序</text>
						<image class="b_img" src="../../static/images/select.png"/>
			</view>
			<view class="lines"></view>
			<view class="item_boxn" @click="cancel()">
				<text class="b_name">按名称排序</text>
					 <image class="b_img" src="../../static/images/circular.png"/>
			</view>
		</view>
	</view>
</template>

<script>
	export default{
		data(){
			return{
				isshow:true,
				
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
		z-index: 99999;
	}
	.b_title{
		text-align: center;
		margin-top: 54upx;
		font-size:34upx;
		font-family:NotoSansHans;
		font-weight:500;
		color:rgba(40,40,40,1);
		line-height:51upx;
	}
	.b_time{
	
		margin-left: 60upx;
		font-size:34upx;
		font-family:NotoSansHans;
		font-weight:400;
		color:rgba(40,40,40,1);
	}
	.b_name{
		
		margin-left: 60upx;
		font-size:34upx;
		font-family:NotoSansHans;
		font-weight:400;
		color:rgba(40,40,40,1);
	}
	.b_img{
		width: 32upx;
		height: 32upx;
		position: relative;
		right: 60upx;
	}

	.item_boxt{
		margin-top: 70upx;
		/* background-color: #ccc; */
		display: flex;
		justify-content: space-between;
		align-items: center;
	}	
	.item_boxn{
		margin-top: 50upx;
		/* background-color: pink; */
		display: flex;
		justify-content: space-between;
		align-items: center;
	}
	.lines{
		width: 86%;
		margin: 50upx auto;
		display: flex;
		border: 0.5px solid #CCCCCC;
	}


</style>
