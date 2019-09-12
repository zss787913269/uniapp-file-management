<template>
	<view>
		<view class="b_box">
			<view class="list">
				<view class="item">下载</view>
				<view class="item">移动</view>
				<view class="item">重命名</view>
				<view class="del">删除</view>
			</view>
		</view>
		<view class="b_box1">
			<view class="list">
				<view class="item1">下载</view>
				<view class="item1">移动</view>
				<view class="item1">重命名</view>
				<view class="del1">删除</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default{
		data(){
			return {
				
			}
		},
		methods:{
			
		}
	}
</script>

<style>
	.b_box {
		position: fixed;
		bottom: 0;
		border-top: 0.5px solid #ccc;
		background: rgba(255, 255, 255, 0.7);
		width: 750upx;
	}
		.b_box1 {
		position: fixed;
		bottom: 0upx;
		border-top: 0.5px solid #ccc;
		background: rgba(255, 255, 255, 0.7);
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
