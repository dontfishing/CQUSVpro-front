<template>
	<view class="myself">
		<view class="row1">
			<view class="myPassage" @click="gotoMyP()">
				<image src="../../static/passage.png" style="height: 60px; width: 70px;"></image>
				</br><text>我的文章</text>
			</view>
			<view class="myComment" @click="gotoMyC() ">
				<image src="../../static/comment.png" style="height: 60px; width: 70px;"></image>
				<!-- <u--text text="我的评论"></u--text> -->
				</br>
				<text class="comText">我的评论</text>
			</view>

		</view>
		<view class="row2">
			<view class="writePassage" @click="gotoMyW()">
				<image src="../../static/edit.png" style="height: 60px; width: 70px;"></image>
				</br><text>发表文章</text>
			</view>
			<view class="writeMood" @click="gotoMood()">
				<image src="../../static/mood_2.png" style="height: 60px; width: 70px;"></image>
				</br><text>发布心情</text>
			</view>

		</view>
	</view>
</template>

<script>
	export default {
		// data() : {
		// 	return 
		// }

		methods: {
			gotoMyP() {
				uni.navigateTo({
					url: 'myPassage'
				})
			},

			gotoMyC() {
				uni.navigateTo({
					url: 'myComment'
				})
			},

			gotoMyW() {
				uni.navigateTo({
					url: 'passageWriting'
				})
			},
			gotoMood() {
				uni.navigateTo({
					url: '/pages/mood/moodWriting'
				});
			}
		}

	}
</script>

<style>
	.myself {
		display: flex;
		flex-direction: column;

	}

	.row1 {
		display: flex;
		flex-direction: row;
	}

	.row2 {
		display: flex;
		flex-direction: row;

	}

	.myPassage {
		background-color: #c3b4e4;
		padding: 10% 10% 10% 10%;
		margin: 10% 5% 10% 5%;
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;

	}

	.myComment {
		background-color: #d3d6f6;
		padding: 10% 10% 10% 10%;
		margin: 10% 5% 10% 5%;
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
	}

	.writePassage {
		background-color: #a5bae3;
		padding: 10% 10% 10% 10%;
		margin: 10% 5% 10% 5%;
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
	}

	.writeMood {
		background-color: rgb(212, 213, 208);
		padding: 10% 10% 10% 10%;
		margin: 10% 5% 10% 5%;
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;

	}

	.comTextz {
		padding: 5% 6% 6% 6%;
		font-size: 20px;
		box-sizing: 100px,100px;
	}

	image {
		padding: 5% 5% 6% 6%;
	}

	view {
		border-radius: 5%;
	}

	text {
		padding: 5% 6% 6% 6%;
		font-size: 20px;
		width: 80px;
		height: 50rpx;
	}
</style>
