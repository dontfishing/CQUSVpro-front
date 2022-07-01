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
			<audio style="text-align: left" :src="current.src" :poster="current.poster" :loop="false"
				:action="audioAction" :name="current.name" controls @play="play()"></audio>
		</view>

		<!-- 文章内容 -->
		<view class="passageContent" scroll-y="true">
			<!-- <text selectable="true">{{psgContent}}</text> -->
			<u--text :text="psgContent" margin="1% 1% 1% 1%"></u--text>
		</view>
		<!-- 点赞、点踩区 -->
		<view class="LikesAndDisl">
			<view class="Like">
				<u-icon :name="dislikeIcon" size="25" @click="clickDislikeIcon()"></u-icon>
				<!-- <u--text :text="likeSum" size="14" color="rgb(100,100,100)"></u--text> -->
			</view>
			<view>
				<u-icon :name="likeIcon" :label="likeSum" labelSize="14" labelColor="rgb(100,100,100)" size="25"
					@click="clickLikeIcon()"></u-icon>
			</view>
		</view>

		<!-- 评论区 -->
		<view v-for="(item, index) in commentList" :key="index">
			<uni-card :title="commentList[index].cmtAuthorName" :sub-title="commentList[index].cmtTime"
				:thumbnail="commentList[index].cmtAuthorImg">
				<!-- 评论 -->
				<u-read-more>
					<rich-text :nodes=" commentList[index].cmtContent">
					</rich-text>
				</u-read-more>

				<!-- 语音播放 -->
				<view class="player">
					<audio style="text-align: left" :src="commentList[index].cmtTts" :loop="false"
						:poster="commentList[index].poster" controls @play="play()"></audio>
				</view>

				<!-- 点赞踩栏 -->
				<view class="LikesAndDisl">
					<u-icon :name="commentList[index].cmtDisLikeState ?'thumb-down-fill':'thumb-down'" size="25"
						@click="clickComDislikeIcon(index)"></u-icon>
					<u-icon :name="commentList[index].cmtLikeState?'thumb-up-fill':'thumb-up'" size="25"
						@click="clickComLikeIcon(index)"></u-icon>
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
				token: '',
				postID: 1,
				portraitSrc: "", // 头像来源
				userName: "", //用户名
				submitTime: "", //提交时间
				current: { //当前文章音频信息
					src: '', //音频来源
					poster: 'https://bjetxgzv.cdn.bspapp.com/VKCEYUGU-uni-app-doc/7fbf26a0-4f4a-11eb-b680-7980c8a877b8.png', //音频图片
					name: "", //文章标题
					// loop: true, //是否循环
				},
				audioAction: {
					method: 'pause'
				},
				psgContent: "",
				likeIcon: "thumb-up",
				likeSum: 0,
				dislikeIcon: "thumb-down",
				infoAmount: 0,
				commentList: [],
				admin: false
			}
		},
		onLoad: function() {
			let that = this;
			that.getPassageInfo();
			let isAdmin = uni.getStorageSync('admin');
			that.admin = isAdmin;
		},
		methods: {
			// 获取文章信息，包括头像、用户名、时间、音频
			getPassageInfo() {
				// 获取缓存中的postID
				let _this = this;
				let postID = uni.getStorageSync('postID');
				let Token = uni.getStorageSync('login_token');
				this.token = Token;
				this.postID = postID;
				uni.request({ // 获取文章
					url: 'http://106.14.62.110:8080/essay/detail', //api地址
					method: "POST",
					data: {
						postId: postID,
						token: Token
					},
					success: res => {
						if (res.statusCode == 404) { //返回的状态码
							uni.showToast({
								icon: 'error',
								title: '网页失踪了',
							});
						} else {
							let infoAmount = res.data["infoAmount"]; // 评论数量
							this.current.name = res.data["postTitle"]
							this.userName = res.data["userName"]; // 用户名
							this.portraitSrc = res.data["userImg"]; // 头像
							this.submitTime = res.data["postTime"]; // 时间
							this.current.src = res.data["postTts"]; // 获音频
							this.current.poster = res.data["userImg"]; // 头像
							this.psgContent = res.data["postContent"]; // 文章内容
							this.likeSum = res.data["postLike"] //点赞数
							this.likeIcon = res.data["likeState"] ? "thumb-up-fill" : "thumb-up" // 文章点赞状态
							this.dislikeIcon = res.data["dislikeState"] ? "thumb-down-fill" :
								"thumb-down" // 文章点踩状态
							if (infoAmount > 0) { //加载评论1
								var tmp1 = {
									cmtId: 123,
									cmtContent: "",
									cmtTts: "",
									cmtAuthorName: "",
									cmtAuthorImg: '',
									cmtTime: '',
									cmtLikeState: false,
									cmtDisLikeState: false,
									poster: 'https://bjetxgzv.cdn.bspapp.com/VKCEYUGU-uni-app-doc/7fbf26a0-4f4a-11eb-b680-7980c8a877b8.png'
								};
								tmp1.cmtId = res.data["cmtId1"];
								tmp1.cmtContent = res.data["cmtContent1"];
								tmp1.cmtTts = res.data["cmtTts1"];
								tmp1.cmtAuthorName = res.data["cmtAuthorName1"];
								tmp1.cmtAuthorImg = res.data["cmtAuthorImg1"];
								tmp1.cmtTime = res.data["cmtTime1"];
								tmp1.cmtLikeState = res.data["cmtLikeState1"];
								tmp1.cmtDisLikeState = res.data["cmtDisLikeState1"];
								_this.commentList.push(tmp1);
							}
							if (infoAmount > 1) { //加载评论2
								var tmp2 = {
									cmtId: 123,
									cmtContent: "",
									cmtTts: "",
									cmtAuthorName: "",
									cmtAuthorImg: '',
									cmtTime: '',
									cmtLikeState: false,
									cmtDisLikeState: false,
									poster: 'https://bjetxgzv.cdn.bspapp.com/VKCEYUGU-uni-app-doc/7fbf26a0-4f4a-11eb-b680-7980c8a877b8.png'
								};
								tmp2.cmtId = res.data["cmtId2"];
								tmp2.cmtContent = res.data["cmtContent2"];
								tmp2.cmtTts = res.data["cmtTts2"];
								tmp2.cmtAuthorName = res.data["cmtAuthorName2"];
								tmp2.cmtAuthorImg = res.data["cmtAuthorImg2"];
								tmp2.cmtTime = res.data["cmtTime2"];
								tmp2.cmtLikeState = res.data["cmtLikeState2"];
								tmp2.cmtDisLikeState = res.data["cmtDisLikeState2"];
								_this.commentList.push(tmp2);
							}
							if (infoAmount > 2) { //加载评论3
								var tmp3 = {
									cmtId: 123,
									cmtContent: "",
									cmtTts: "",
									cmtAuthorName: "",
									cmtAuthorImg: '',
									cmtTime: '',
									cmtLikeState: false,
									cmtDisLikeState: false,
									poster: 'https://bjetxgzv.cdn.bspapp.com/VKCEYUGU-uni-app-doc/7fbf26a0-4f4a-11eb-b680-7980c8a877b8.png'
								};
								tmp3.cmtId = res.data["cmtId3"];
								tmp3.cmtContent = res.data["cmtContent3"];
								tmp3.cmtTts = res.data["cmtTts3"];
								tmp3.cmtAuthorName = res.data["cmtAuthorName3"];
								tmp3.cmtAuthorImg = res.data["cmtAuthorImg3"];
								tmp3.cmtTime = res.data["cmtTime3"];
								tmp3.cmtLikeState = res.data["cmtLikeState3"];
								tmp3.cmtDisLikeState = res.data["cmtDisLikeState3"];
								_this.commentList.push(tmp3);
							}
							// _this.postID = postID;
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
			play() {},
			clickLikeIcon() { // 点击文章点赞按钮
				if (this.likeIcon == "thumb-up-fill") { //取消点赞
					uni.request({
						url: 'http://106.14.62.110:8080/post/like/cancel', //api地址
						method: "POST",
						data: {
							postId: this.postID,
							token: this.token

						},
						success: res => {
							if (res.statusCode == 404) { //返回的状态码
								uni.showToast({
									icon: 'error',
									title: '网页失踪了',
								});
							} else if ('erro' in res.data) {
								uni.showToast({
									title: 'error'
								})
							} else {
								this.likeIcon = "thumb-up";
								this.likeSum--;
							}
						},
						fail: () => {

						}
					});
				} else { //点赞
					uni.request({
						url: 'http://106.14.62.110:8080/post/like', //api地址
						method: "POST",
						data: {
							postId: this.postID,
							token: this.token
						},
						success: res => {
							console.log("点赞");
							console.log(res.data);

							if (res.statusCode == 404) { //返回的状态码
								uni.showToast({
									icon: 'error',
									title: '网页失踪了',
								});
							} else if ('erro' in res.data) {
								uni.showToast({
									title: 'error'
								})
							} else {
								this.likeIcon = "thumb-up-fill";
								this.likeSum++;
							}
						},
						fail: () => {

						}
					});
					uni.request({
						url: 'http://106.14.62.110:8080/post/dislike/cancel', //api地址
						method: "POST",
						data: {
							postId: this.postID,
							token: this.token
						},
						success: res => {
							// console.log(res.data);
							if (res.statusCode == 404) { //返回的状态码
								uni.showToast({
									icon: 'error',
									title: '网页失踪了',
								});
							} else if ('erro' in res.data) {
								uni.showToast({
									title: 'error'
								})
							} else {
								this.dislikeIcon = "thumb-down";
							}
						},
						fail: () => {

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
							postId: this.postID,
							token: this.token
						},
						success: res => {
							console.log(res.data);
							if (res.statusCode == 404) { //返回的状态码
								uni.showToast({
									icon: 'error',
									title: '网页失踪了',
								});
							} else if ('erro' in res.data) {
								uni.showToast({
									title: 'error'
								})
							} else {
								this.dislikeIcon = "thumb-down";
							}
						},
						fail: () => {

						}
					});
				} else { //点踩
					uni.request({
						url: 'http://106.14.62.110:8080/post/dislike', //api地址
						method: "POST",
						data: {
							postId: this.postID,
							token: this.token
						},
						success: res => {
							console.log(res.data);
							if (res.statusCode == 404) { //返回的状态码
								uni.showToast({
									icon: 'error',
									title: '网页失踪了',
								});
							} else if ('erro' in res.data) {
								uni.showToast({
									title: 'error'
								})
							} else {
								this.dislikeIcon = "thumb-down-fill";
							}
						},
						fail: () => {

						}
					});
					if (this.likeIcon == "thumb-up-fill") {
						uni.request({
							url: 'http://106.14.62.110:8080/post/like/cancel', //api地址
							method: "POST",
							data: {
								postId: this.postID,
								token: this.token
							},
							success: res => {
								console.log(res.data);
								if (res.statusCode == 404) { //返回的状态码
									uni.showToast({
										icon: 'error',
										title: '网页失踪了',
									});
								} else if ('erro' in res.data) {
									uni.showToast({
										title: 'error'
									})
								} else {
									this.likeIcon = "thumb-up";
									this.likeSum--;
								}
							},
							fail: () => {

							}
						});
					}
				}
			},
			clickComLikeIcon(index) { // 点击评论点赞按钮
				if (this.commentList[index].cmtLikeState) { // 取消点赞
					uni.request({
						url: 'http://106.14.62.110:8080/comment/like/cancel', //api地址
						method: "POST",
						data: {
							cmtId: this.commentList[index]["cmtId"],
							token: this.token
						},
						success: res => {
							console.log(res.data);
							if (res.statusCode == 404) { //返回的状态码
								uni.showToast({
									icon: 'error',
									title: '网页失踪了',
								});
							} else if ('erro' in res.data) {
								uni.showToast({
									title: 'error'
								})
							} else {
								this.commentList[index].cmtLikeState = false;
							}
						},

						fail: () => {

						}
					});

				} else { // 评论点赞
					uni.request({
						url: 'http://106.14.62.110:8080/comment/like', //api地址
						method: "POST",
						data: {
							cmtId: this.commentList[index]["cmtId"],
							token: this.token
						},
						success: res => {
							console.log(res.data);
							if (res.statusCode == 404) { //返回的状态码
								uni.showToast({
									icon: 'error',
									title: '网页失踪了',
								});
							} else if ('erro' in res.data) {
								uni.showToast({
									title: 'error'
								})
							} else {
								this.commentList[index].cmtLikeState = true;

							}
						},

						fail: () => {

						}
					});
					if (this.commentList[index].cmtDisLikeState == true) {
						uni.request({
							url: 'http://106.14.62.110:8080/comment/dislike/cancel', //api地址
							method: "POST",
							data: {
								cmtId: this.commentList[index]["cmtId"],
								token: this.token
							},
							success: res => {
								console.log(res.data);
								if (res.statusCode == 404) { //返回的状态码
									uni.showToast({
										icon: 'error',
										title: '网页失踪了',
									});
								} else if ('erro' in res.data) {
									uni.showToast({
										title: 'error'
									})
								} else {
									this.commentList[index].cmtDisLikeState = false;
								}
							},

							fail: () => {

							}
						});
					}
				}
			},
			clickComDislikeIcon(index) { //评论点击点踩
				if (this.commentList[index].cmtDisLikeState) { //取消点踩
					uni.request({
						url: 'http://106.14.62.110:8080/comment/dislike/cancel', //api地址
						method: "POST",
						data: {
							cmtId: this.commentList[index]["cmtId"],
							token: this.token
						},
						success: res => {
							console.log(res.data);
							if (res.statusCode == 404) { //返回的状态码
								uni.showToast({
									icon: 'error',
									title: '网页失踪了',
								});
							} else if ('erro' in res.data) {
								uni.showToast({
									title: 'error'
								})
							} else {
								this.commentList[index].cmtDisLikeState = false;
							}
						},

						fail: () => {

						}
					});

				} else { //点踩
					uni.request({
						url: 'http://106.14.62.110:8080/comment/dislike', //api地址
						method: "POST",
						data: {
							cmtId: this.commentList[index]["cmtId"],
							token: this.token
						},
						success: res => {
							console.log(res.data);
							if (res.statusCode == 404) { //返回的状态码
								uni.showToast({
									icon: 'error',
									title: '网页失踪了',
								});
							} else if ('erro' in res.data) {
								uni.showToast({
									title: 'error'
								})
							} else {
								this.commentList[index].cmtDisLikeState = true;
							}
						},

						fail: () => {

						}
					});
					if (this.commentList[index].cmtLikeState == true) {
						uni.request({
							url: 'http://106.14.62.110:8080/comment/like/cancel', //api地址
							method: "POST",
							data: {
								cmtId: this.commentList[index]["cmtId"],
								token: this.token
							},
							success: res => {
								console.log(res.data);
								if (res.statusCode == 404) { //返回的状态码
									uni.showToast({
										icon: 'error',
										title: '网页失踪了',
									});
								} else {
									this.commentList[index].cmtLikeState = false;
								}
							},

							fail: () => {

							}
						});
					}
				}
			},
			getCom(index) { //获取评论
				let _this = this;
				let postTime = _this.commentList[index].cmtTime;
				let postID = uni.getStorageSync('postID');
				let Token = uni.getStorageSync('login_token');
				this.postID = postID;
				this.token = Token;
				uni.request({
					url: 'http://106.14.62.110:8080/essay/detail/refresh', //api地址
					method: "POST",
					data: {
						postId: postID,
						time: postTime,
						token: Token
					},
					success: (res) => {
						if (res.data.infoAmount == 0) {
							uni.showToast({
								title: "已经没有更多评论了，快去评论吧！"
							})
						}
						if (res.data.infoAmount > 0) { //加载评论1
							var tmp1 = {
								cmtId: 123,
								cmtContent: "",
								cmtTts: "",
								cmtAuthorName: "",
								cmtAuthorImg: '',
								cmtTime: '',
								cmtLikeState: false,
								cmtDisLikeState: false,
								poster: 'https://bjetxgzv.cdn.bspapp.com/VKCEYUGU-uni-app-doc/7fbf26a0-4f4a-11eb-b680-7980c8a877b8.png'
							};
							tmp1.cmtId = res.data["cmtId1"];
							tmp1.cmtContent = res.data["cmtContent1"];
							tmp1.cmtTts = res.data["cmtTts1"];
							tmp1.cmtAuthorName = res.data["cmtAuthorName1"];
							tmp1.cmtAuthorImg = res.data["cmtAuthorImg1"];
							tmp1.cmtTime = res.data["cmtTime1"];
							tmp1.cmtLikeState = res.data["cmtLikeState1"];
							tmp1.cmtDisLikeState = res.data["cmtDisLikeState1"];
							_this.commentList.push(tmp1);
						}
						if (res.data.infoAmount > 1) { //加载评论2
							var tmp2 = {
								cmtId: 123,
								cmtContent: "",
								cmtTts: "",
								cmtAuthorName: "",
								cmtAuthorImg: '',
								cmtTime: '',
								cmtLikeState: false,
								cmtDisLikeState: false,
								poster: 'https://bjetxgzv.cdn.bspapp.com/VKCEYUGU-uni-app-doc/7fbf26a0-4f4a-11eb-b680-7980c8a877b8.png'
							};
							tmp2.cmtId = res.data["cmtId2"];
							tmp2.cmtContent = res.data["cmtContent2"];
							tmp2.cmtTts = res.data["cmtTts2"];
							tmp2.cmtAuthorName = res.data["cmtAuthorName2"];
							tmp2.cmtAuthorImg = res.data["cmtAuthorImg2"];
							tmp2.cmtTime = res.data["cmtTime2"];
							tmp2.cmtLikeState = res.data["cmtLikeState2"];
							tmp2.cmtDisLikeState = res.data["cmtDisLikeState2"];
							_this.commentList.push(tmp2);
						}
						if (res.data.infoAmount > 2) { //加载评论3
							var tmp3 = {
								cmtId: 123,
								cmtContent: "",
								cmtTts: "",
								cmtAuthorName: "",
								cmtAuthorImg: '',
								cmtTime: '',
								cmtLikeState: false,
								cmtDisLikeState: false,
								poster: 'https://bjetxgzv.cdn.bspapp.com/VKCEYUGU-uni-app-doc/7fbf26a0-4f4a-11eb-b680-7980c8a877b8.png'
							};
							tmp3.cmtId = res.data["cmtId3"];
							tmp3.cmtContent = res.data["cmtContent3"];
							tmp3.cmtTts = res.data["cmtTts3"];
							tmp3.cmtAuthorName = res.data["cmtAuthorName3"];
							tmp3.cmtAuthorImg = res.data["cmtAuthorImg3"];
							tmp3.cmtTime = res.data["cmtTime3"];
							tmp3.cmtLikeState = res.data["cmtLikeState3"];
							tmp3.cmtDisLikeState = res.data["cmtDisLikeState3"];
							_this.commentList.push(tmp3);
						}
					}
				})
			},

			Comment() {
				uni.redirectTo({
					url: '/pages/passage/commentWriting'
				});
			},
			onReachBottom() {
				let _this = this;
				let len = _this.commentList.length - 1;
				_this.getCom(len);
			}
		},
	}
</script>

<style>
	page {
		height: initial;
		overflow-y: initial;
		min-height: 100vh;
	}

	/* 文章内容界面 */
	.passageInfo {
		background-color: white;
		/* 		width: 100%;
		height: 100%; */
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

	.Like {
		margin: 0% 2% 0% 2%;
		display: flex;
		flex-direction: row;

	}

	/*  */
	.commentArea {}

	.commentLine {
		position: fixed;
		bottom: 0%;
		justify-content: center;
		align-items: center;
		border: 1px solid #efefef;
		width: 100%;
		border-radius: 1%;
		padding: 1% 2% 1% 2%;
		background-color: white;
	}
</style>
