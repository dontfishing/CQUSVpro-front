<template>
	<view class="whole">
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
		<view class="LikesAndDisl">
			<u-icon :name="likeIcon" size="25" @click="clickLikeIcon()"></u-icon>
			<u-icon :name="dislikeIcon" size="25" @click="clickDislikeIcon()"></u-icon>
		</view>

		<!-- 评论区 -->
		<view v-for="(item, index) in commentList" :key="index">
			<uni-card :title="commentList[index].cmtAuthorName" :sub-title="commentList[index].cmtTime"
				thumbnail="https://vkceyugu.cdn.bspapp.com/VKCEYUGU-dc-site/460d46d0-4fcc-11eb-8ff1-d5dcf8779628.png">
				<!-- 评论 -->
				<u-read-more>
					<rich-text :nodes=" commentList[index].cmtContent">
					</rich-text>
				</u-read-more>

				<!-- 语音播放 -->
				<view class="player">
					<audio style="text-align: left" :src="commentList[index].cmtTts" :poster="commentList[index].cmtAuthorImg"
						controls @play="play()"></audio>
				</view>

				<!-- 点赞踩栏 -->
				<view class="LikesAndDisl">
					<u-icon :name="commLikeIcon" size="25" @click="clickComLikeIcon(index)"></u-icon>
					<u-icon :name="commDislikeIcon" size="25" @click="clickComDislikeIcon(index)"></u-icon>
				</view>
			</uni-card>
		</view>

		<!-- 评论栏 -->
		<view class="commentLine">
			<u-icon name="chat" size="25px" label="文明评论,从我做起" labelColor="rgb(198,198,198)" @click="Comment()"></u-icon>
		</view>

	</view>
</template>

<script>
	export default {
		data() {
			return {
				postID: 1,
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
				commLikeIcon: "thumb-up",
				commDislikeIcon: "thumb-down",
				infoAmount: 0,
				commentList: [],
			}
		},
		methods: {

			// 获取文章信息，包括头像、用户名、时间、音频
			getPassageInfo() {
				// 获取缓存中的postID
				let _this=this;
				uni.getStorage({ // 获取缓存中的postID
					key: 'postID',
					success: function(res) {
						_this.postID=res.data;
					},
					fail: () => {
						console.log(JSON.stringify(res.data));
						console.log('fail');

					}
				});

				uni.request({ // 获取文章
					url: 'http://106.14.62.110:8080/essay/detail', //api地址
					method: "POST",
					data: {
						postId: _this.postID
					},
					success: res => {
						console.log(res.data);
						
						if (res.statusCode == 404) { //返回的状态码
							uni.showToast({
								icon: 'error',
								title: '网页失踪了',
							});
						} else {
							console.log(res.data);
							this.infoAmount = res.data["infoAmount"]; // 评论数量
							this.userName = res.data["userName"]; // 用户名
							this.portraitSrc = res.data["userImg"]; // 头像
							this.submitTime = res.data["postTime"]; // 时间
							this.current.src = res.data["postTts"]; // 获音频
							this.current.poster = res.data["userImg"]; // 头像
							this.psgContent = res.data["postContent"]; // 文章内容
							if (this.infoAmount > 0) {
								var tmp1 = {
									cmtId: 123,
									cmtContent: "",
									cmtTts: "",
									cmtAuthorName: "",
									cmtAuthorImg: '',
									cmtTime: '',
								};
								tmp1.cmtId=res.data["cmtId1"];
								tmp1.cmtContent=res.data["cmtContent1"];
								tmp1.cmtTts=res.data["cmtTts1"];
								tmp1.cmtAuthorName=res.data["cmtAuthorName1"];
								tmp1.cmtAuthorImg=res.data["cmtAuthorImg1"];
								tmp1.cmtTime=res.data["cmtTime1"];
								this.commentList.push(tmp1);
							}
							console.log(res.data);
							// this.commentList.push(res.data); // 评论1
							// this.commentList.push(res.data["comment2"]); // 评论2
							// this.commentList.push(res.data["comment3"]); // 评论3
						}
					},

					fail: res => {
						console.log(res.data);

					},

					complete: () => {

					}
				});
			},
			// 播放音频
			play() {

			},

			clickLikeIcon() { // 点击文章点赞按钮
				if (this.likeIcon == "thumb-up-fill") { //取消点赞
					uni.request({
						url: 'http://106.14.62.110:8080/post/like/cancel', //api地址
						method: "POST",
						data: {
							postId: this.postID
						},
						success: res => {
							console.log("取消点赞");
							console.log(res.data);
							if (res.statusCode == 404) { //返回的状态码
								uni.showToast({
									icon: 'error',
									title: '网页失踪了',
								});
							} else {
								this.likeIcon = "thumb-up";
							}
						},

						fail: () => {

						},

						complete: () => {

						}
					});

				} else { //点赞
					uni.request({
						url: 'http://106.14.62.110:8080/post/like', //api地址
						method: "POST",
						data: {
							postId: this.postID
						},
						success: res => {
							console.log("点赞");
							console.log(res.data);
							
							if (res.statusCode == 404) { //返回的状态码
								uni.showToast({
									icon: 'error',
									title: '网页失踪了',
								});
							} else {
								this.likeIcon = "thumb-up-fill";
							}
						},

						fail: () => {

						},

						complete: () => {

						}
					});

				}
			},


			clickDislikeIcon() { // 点击文章点踩按钮
				if (this.dislikeIcon == "thumb-down-fill") { //取消点踩
					console.log(this.postID);
					uni.request({
						url: 'http://106.14.62.110:8080/post/dislike/cancel', //api地址
						method: "POST",
						data: {
							postId: this.postID
						},
						success: res => {
							console.log(res.data);
							if (res.statusCode == 404) { //返回的状态码
								uni.showToast({
									icon: 'error',
									title: '网页失踪了',
								});
							} else {
								this.dislikeIcon = "thumb-down";
							}
						},

						fail: () => {

						},

						complete: () => {

						}
					});

				} else { //点踩
					uni.request({
						url: 'http://106.14.62.110:8080/post/dislike', //api地址
						method: "POST",
						data: {
							postId: this.postID
						},
						success: res => {
							console.log(res.data);
							if (res.statusCode == 404) { //返回的状态码
								uni.showToast({
									icon: 'error',
									title: '网页失踪了',
								});
							} else {
								this.dislikeIcon = "thumb-down-fill";
							}
						},

						fail: () => {

						},

						complete: () => {

						}
					});

				}

			},

			clickComLikeIcon(index) { // 点击评论点赞按钮
				if (this.commLikeIcon == "thumb-up-fill") { // 取消点赞
					uni.request({
						url: 'http://106.14.62.110:8080/comment/like/cancel', //api地址
						method: "POST",
						data: {
							cmtId: this.commentList[index][cmtId]
						},
						success: res => {
							console.log(res.data);
							if (res.statusCode == 404) { //返回的状态码
								uni.showToast({
									icon: 'error',
									title: '网页失踪了',
								});
							} else {
								this.commLikeIcon == "thumb-up";
							}
						},

						fail: () => {

						},

						complete: () => {

						}
					});

				} else { // 评论点赞
					uni.request({
						url: 'http://106.14.62.110:8080/comment/like', //api地址
						method: "POST",
						data: {
							cmtId: this.commentList[index][cmtId]
						},
						success: res => {
							console.log(res.data);
							if (res.statusCode == 404) { //返回的状态码
								uni.showToast({
									icon: 'error',
									title: '网页失踪了',
								});
							} else {

							}
						},

						fail: () => {

						},

						complete: () => {

						}
					});

				}
			},

			clickDislikeIcon(index) { //评论点击点踩
				if (this.commDislikeIcon == "thumb-down-fill") { //取消点踩
					uni.request({
						url: 'http://106.14.62.110:8080/comment/dislike/cancel', //api地址
						method: "POST",
						data: {
							cmtId: this.commentList[index][cmtId]
						},
						success: res => {
							console.log(res.data);
							if (res.statusCode == 404) { //返回的状态码
								uni.showToast({
									icon: 'error',
									title: '网页失踪了',
								});
							} else {
								this.dislikeIcon = "thumb-down";
							}
						},

						fail: () => {

						},

						complete: () => {

						}
					});

				} else { //点踩
					uni.request({
						url: 'http://106.14.62.110:8080/comment/dislike', //api地址
						method: "POST",
						data: {
							cmtId: this.commentList[index][cmtId]
						},
						success: res => {
							console.log(res.data);
							if (res.statusCode == 404) { //返回的状态码
								uni.showToast({
									icon: 'error',
									title: '网页失踪了',
								});
							} else {
								this.commDislikeIcon = "thumb-down-fill";
							}
						},

						fail: () => {

						},

						complete: () => {

						}
					});

				}

			},
			// onLoad() { //每次加载都会重新刷新
			// 	let that = this;
			// 	that.getPassageInfo();
			// },
			onLoad: function() {
				let that = this;
				setTimeout(function() {
					console.log('start pulldown');
				}, 1000);
				that.getPassageInfo();
				uni.startPullDownRefresh();

			},
			onPullDownRefresh() {
				console.log('refresh');
				setTimeout(function() {
					uni.stopPullDownRefresh();
				}, 1000);
			},
			loadmore() {
				this.loadMoreStatus = 'loading';
			},
			Comment() {
				uni.navigateTo({
					url: '/pages/passage/commentWriting'
				});
			},

		},

	}
</script>

<style>
	/* 文章内容界面 */
	.passageInfo {
		background-color: white;
		width: 100%;
		height: 100%;
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
		align-items: center;
	}

	/* 文章内容部分 */
	.passageContent {
		border: 1px solid #f5f5f5;
		margin: 2% 2% 2% 2%;
		border-radius: 3%;
		justify-content: center;
		align-items: center;

	}

	/* 评论、点赞、点踩区 */
	.LikesAndDisl {
		display: flex;
		flex-direction: row-reverse;
		margin: 2% 2% 2% 2%;

	}

	/*  */
	.commentArea {}

	.commentLine {
		position: absolute;
		justify-content: center;
		align-items: center;
		border: 1px solid #efefef;
		width: 100%;
		border-radius: 1%;
		padding: 1% 2% 1% 2%;
	}
</style>
