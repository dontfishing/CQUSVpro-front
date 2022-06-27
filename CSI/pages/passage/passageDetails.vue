<template>
	<view>
		<!-- 文章信息 -->
		<view class="passageInfo">
			<!-- 头像 -->
			<view class="portrait">
				<image :src="portraitSrc" style="height: 60px; width: 60px;"></image>
			</view>

			<!-- 用户名与时间 -->
			<view class="userNameAndTime">
				<!-- 用户名 -->
				<u--text :text="userName" size="18" margin="10px 0% 5px 15%" bold></u--text>
				<!-- 时间 -->
				<u--text :text="submitTime" size="16" margin="5px 0% 10px 15%"></u--text>
			</view>
		</view>

		<!-- 语音播放 -->
		<view class="player">
			<audio style="text-align: left" :src="current.src" :poster="current.poster" loop=true :action="audioAction"
				controls @play="play()"></audio>
		</view>

		<!-- 文章内容 -->
		<view class="passageContent" scroll-y="true">
			<!-- <text selectable="true">{{psgContent}}</text> -->
			<u--text :text="psgContent" margin="1% 1% 1% 1%"></u--text>
		</view>

		<!-- 点赞、点踩区 -->
		<view class="comAndLikesAndDisl">
			<u-icon :name="likeIcon" size="25" @click="clickLikeIcon()"></u-icon>
			<u-icon :name="dislikeIcon" size="25" @click="clickDislikeIcon()"></u-icon>
		</view>


	</view>
</template>

<script>
	export default {
		data() {
			return {
				postID: "123",
				portraitSrc: "https://bjetxgzv.cdn.bspapp.com/VKCEYUGU-uni-app-doc/7fbf26a0-4f4a-11eb-b680-7980c8a877b8.png", // 头像来源
				userName: "用户名", //用户名
				submitTime: "时间", //提交时间
				current: { //当前文章音频信息
					src: 'https://bjetxgzv.cdn.bspapp.com/VKCEYUGU-hello-uniapp/2cc220e0-c27a-11ea-9dfb-6da8e309e0d8.mp3', //音频来源
					poster: 'https://bjetxgzv.cdn.bspapp.com/VKCEYUGU-uni-app-doc/7fbf26a0-4f4a-11eb-b680-7980c8a877b8.png', //音频图片
					// loop: true, //是否循环
				},
				audioAction: {
					method: 'pause'
				},
				psgContent: "待定\n内容\n好丑",
				likeIcon: "thumb-up",
				dislikeIcon: "thumb-down",
			}
		},
		methods: {
			// 获取文章信息，包括头像、用户名、时间、音频
			getPassageInfo() {
				uni.request({
					url: 'http://106.14.62.110:8080/essay/detail', //api地址
					method: "POST",
					data: {
						postID: this.postID
					},
					success: res => {
						if (res.statusCode == 404) { //返回的状态码
							uni.showToast({
								icon: 'error',
								title: '网页失踪了',
							});
						} else {
							this.userName = res.data["userName"];
							this.portraitSrc = res.data["userImg"];
							this.submitTime = res.data["postTime"];
							this.current.src = res.data["postTts"];
							this.current.poster = res.data["userImg"];
							this.psgContent = res.data["postContent"];
						}
					},

					fail: () => {

					},

					complete: () => {

					}
				});
			},
			// 播放音频
			play() {

			},
			// 点击点赞按钮
			clickLikeIcon() {
				if (this.likeIcon == "thumb-up-fill") {
					this.likeIcon = "thumb-up";
				} else {
					this.likeIcon = "thumb-up-fill";
				}
			},

			// 点击点踩按钮
			clickDislikeIcon() {
				if (this.dislikeIcon == "thumb-down-fill") {
					this.dislikeIcon = "thumb-down";
				} else {
					this.dislikeIcon = "thumb-down-fill";
				}
			},
			onload() { //加载时获取数据
				uni.getStorage({
					key: 'passage_detail',
					success: function(res) {
						console.log(res.data);
					}
				})
			},
			loadmore() {
				this.loadMoreStatus = 'loading';
			}


		},

	}
</script>

<style>
	/* 文章内容界面 */
	.passageInfo {
		background-color: white;
		display: flex;
		flex-direction: row;
	}

	/* 头像 */
	.portrait {
		margin: 2% 2% 2% 5%;
	}

	/* 用户名和提交时间部分 */
	.userNameAndTime {}

	/* 音频播放部分 */
	.player {
		/* margin: 5% 2% 5% 2%; */
		display: flex;
		flex-direction: row;
		justify-content: center;

	}

	/* 文章内容部分 */
	.passageContent {
		border: 1px solid #f5f5f5;
		margin: 2% 2% 2% 2%;
		border-radius: 3%;
	}

	/* 评论、点赞、点踩区 */
	.comAndLikesAndDisl {
		display: flex;
		flex-direction: row-reverse;
		margin: 2% 2% 2% 2%;

	}

	/*  */
	.commentArea {}
</style>
