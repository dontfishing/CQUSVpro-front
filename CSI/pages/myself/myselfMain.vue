<template>
	<view>
		<view class="myPassage" @click="gotoMyP()">
			<image src="../../static/文章.png" style="height: 70px; width: 80px;"></image>
			<text>我的文章</text>
		</view>
		<view class="myComment" @click="gotoMyC() ">
			<image src="../../static/评论.png" style="height: 70px; width: 80px;"></image>
			<text>我的评论</text>
		</view>
		<view class="writePassage" @click="gotoMyW()">
			<image src="../../static/编辑.png" style="height: 70px; width: 80px;"></image>
			<text>写文章</text>
		</view>
	</view>
</template>

<script>
	export default {
		// data() : {
		// 	return 
		// }
		
		methods : {
			gotoMyP () {
				uni.navigateTo({
					url: 'myPassage'
				})
			},
			
			gotoMyC () {
				uni.navigateTo({
					url: 'myComment'
				})				
			},
			
			gotoMyW () {
				uni.navigateTo({
					url: 'writePassage'
				})
			}
		}
		
	}
</script>

<style>
	.myPassage {
		background-color: #a0dce4;
		margin-top: 5%;
		margin-left: 2%;
		margin-right: 2%;
		display: flex;
		flex-direction: row;
	}
	
	.myComment {
		background-color: #a3bb89;
		margin-top: 5%;
		margin-left: 2%;
		margin-right: 2%;
		display: flex;
		flex-direction: row;
	}
	
	.writePassage {
		background-color: #9fad90;
		margin-top: 5%;
		margin-left: 2%;
		margin-right: 2%;
		display: flex;
		flex-direction: row;
	}
	
	image {
		margin-top: 15%;
		margin-bottom: 15%;
		margin-right: 15%;
		margin-left: 15%;
	}
	
	view {
		border-radius: 5%;
	}
	
	text {
		font-size: 30px;
		margin-top: 20%;
		margin-right: 5%;
	}
</style>
