<template>
	<view>
		<!-- 心情详情 -->
		<view class="moodInfo">
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
		<!-- 状态描述 -->
		<view class="passageContent" scroll-y="true">
			<u--text :text="moodContent" margin="1% 1% 5% 4%" size="18px"></u--text>
		</view>
		<!-- 图片 -->
		<view>
			<image :src="imgSrc" mode="aspectFit"></image>
		</view>
		<!--tag部分-->
		<view class="tags">
			<u-tag class="miniTag" v-for="(item, index) in tagList" :key="index" :text="tagList[index]" size="mini"
				plain borderColor="#909399" color="#909399" shape="circle"></u-tag>
		</view>
		<!--评论区-->
		<view v-for="(item, index) in commentList" :key="index">
			<uni-card :title="commentList[index].cmtAuthorName" :sub-title="commentList[index].cmtTime"
				:thumbnail="commentList[index].cmtAuthorImg">
				<!-- 评论 -->
				<u-read-more>
					<rich-text :nodes=" commentList[index].cmtContent">
					</rich-text>
				</u-read-more>
				<!-- 评论点赞栏 -->
				<view class="LikesAndDisl">
					<u-icon :name="commentList[index].cmtLikeState?'thumb-up-fill':'thumb-up'" size="25"
						@click="clickComLikeIcon(index)"></u-icon>
				</view>
			</uni-card>
		</view>
		<!--底部栏的点赞和评论-->
		<view class="likeAndCmt">
			<view class="cmt" @click="comment()">
				<u-icon name="chat" size="20px"></u-icon>
				<text class="cmtText">评论</text>
			</view>
			<view class="thumb" @click="thumb()">
				<u-icon :name="likeIcon" size="20px"></u-icon>
				<text class="thbText">点赞</text>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				portraitSrc: '', //头像
				userName: '', //用户名
				submitTime: '', //发布时间
				moodContent: '', //心情内容
				imgSrc: '', //心情图片
				tagList: [], //标签列表
				commentList: [], //评论内容
				likeIcon: "thumb-up", //点赞图标的状态，默认是未点赞的
				likeSum: 0, //点赞数
				moodID: 0, //心情ID
				token: '',
			}
		},

		onLoad: function() { //进入页面时加载
			let that = this;
			that.getMoodInfo();
		},

		methods: {
			comment() { //点击评论，进入写评论界面
				uni.navigateTo({
					url: '/pages/mood/moodCmtWrite'
				})
			},

			thumb() { //心情点赞
				if (this.likeIcon == "thumb-up-fill") { //若是已经点赞了,就取消点赞
					uni.request({
						url: 'http://106.14.62.110:8080/mood/dislike', //api地址
						method: "POST",
						data: {
							token: this.token,
							moodId: this.moodID
						},
						success: res => {
							console.log(res.data);
							if (res.statusCode == 404) { //返回的状态码
								uni.showToast({
									icon: 'error',
									title: '网页失踪了',
								});
							} else if ("error" in res.data) {
								uni.showToast({
									icon: 'error',
									title: "error"
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
						url: 'http://106.14.62.110:8080/mood/like', //api地址
						method: "POST",
						data: {
							token: this.token,
							moodId: this.moodID
						},
						success: res => {
							console.log("点赞");
							console.log(res.data);
							if (res.statusCode == 404) { //返回的状态码
								uni.showToast({
									icon: 'error',
									title: '网页失踪了',
								});
							} else if ("error" in res.data) {
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
				}
			},

			getMoodInfo() { //加载时获得页面信息
				// 获取缓存中的postID
				let _this = this;
				let moodID = uni.getStorageSync('moodId');
				let Token = uni.getStorageSync('login_token');
				this.token = Token;
				this.moodID = moodID;
				uni.request({
					url: 'http://106.14.62.110:8080/mood/detail',
					method: "POST",
					data: {
						moodId: moodID,
						token: Token
					},
					success: (res) => {
						console.log(res.data);
						if (res.statusCode == 404) {
							uni.showToast({
								icon: "none",
								title: "网页失踪了"
							})
						} else {
							let infoAmount = res.data["infoAmount"]; // 评论数量
							this.userName = res.data["userName"]; // 用户名
							this.portraitSrc = res.data["userImg"]; // 头像
							this.submitTime = res.data["moodTime"]; // 时间
							this.moodContent = res.data["moodContent"]; // 文章内容
							this.likeSum = res.data["moodLike"]; //点赞数
							this.imgSrc = res.data.moodImg; //心情图片
							console.log(res.data["moodLikeState"]);
							this.likeIcon = res.data["moodLikeState"] ? "thumb-up-fill" : "thumb-up" // 文章点赞状态
							if (infoAmount > 0) {
								var tmp1 = {
									cmtId: res.data.cmtId1,
									cmtAuthorName: res.data.cmtAuthorName1,
									cmtAuthorImg: res.data.cmtAuthorImg1,
									cmtTime: res.data.cmtTime1,
									cmtContent: res.data.cmtContent1,
									cmtLike: res.data.cmtLike1,
									cmtLikeState: res.data.cmtLikeState1
								}
								_this.commentList.push(tmp1);
							}
							if (infoAmount > 1) {
								var tmp2 = {
									cmtId: res.data.cmtId2,
									cmtAuthorName: res.data.cmtAuthorName2,
									cmtAuthorImg: res.data.cmtAuthorImg2,
									cmtTime: res.data.cmtTime2,
									cmtContent: res.data.cmtContent2,
									cmtLike: res.data.cmtLike2,
									cmtLikeState: res.data.cmtLikeState2
								}
								_this.commentList.push(tmp2);
							}
							if (infoAmount > 2) {
								var tmp3 = {
									cmtId: res.data.cmtId3,
									cmtAuthorName: res.data.cmtAuthorName3,
									cmtAuthorImg: res.data.cmtAuthorImg3,
									cmtTime: res.data.cmtTime3,
									cmtContent: res.data.cmtContent3,
									cmtLike: res.data.cmtLike3,
									cmtLikeState: res.data.cmtLikeState3
								}
								_this.commentList.push(tmp3);
							}

							if (res.data.moodTagMood == "life") {
								this.tagList.push("life");
							} else {
								if(res.data.moodTagAge != "")
									_this.tagList.push(res.data.moodTagAge);
								if(res.data.moodTagLevel != "")
									_this.tagList.push(res.data.moodTagLevel);
								if(res.data.moodDetailMood != "")
									_this.tagList.push(res.data.moodDetailMood);
								if(res.data.moodTagShape != "")
									_this.tagList.push(res.data.moodTagShape);
							}
						}
					}
				})
			},

			clickComLikeIcon(index) { // 点击评论点赞按钮
				if (this.commentList[index].cmtLikeState) { // 取消点赞
					uni.request({
						url: 'http://106.14.62.110:8080/mood/comment/like/cancel', //api地址
						method: "POST",
						data: {
							token: this.token,
							moodId: this.moodID	
						},
						success: res => {
							console.log(res.data);
							if (res.statusCode == 404) { //返回的状态码
								uni.showToast({
									icon: 'error',
									title: '网页失踪了',
								});
							} else if ("error" in res.data) {
								uni.showToast({
									icon:"none",
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
						url: 'http://106.14.62.110:8080/mood/comment/like', //api地址
						method: "POST",
						data: {
							token: this.token,
							moodId: this.moodID		
						},
						success: res => {
							console.log(res.data);
							if (res.statusCode == 404) { //返回的状态码
								uni.showToast({
									icon: 'error',
									title: '网页失踪了',
								});
							} else if ("error" in res.data) {
								uni.showToast({
									icon:"error",
									title: 'error'
								})
							} else {
								this.commentList[index].cmtLikeState = true;

							}
						},

						fail: () => {

						}
					});
				}
			},

			getComment(index) { //获取评论
				let _this = this;
				let moodTime = _this.commentList[index].cmtTime;
				let moodID = uni.getStorageSync('moodId');
				let Token = uni.getStorageSync('login_token');
				this.moodID = moodID;
				this.token = Token;
				uni.request({
					url: 'http://106.14.62.110:8080/mood/detail/refresh',
					method: "POST",
					data: {
						moodId: moodID,
						time: moodTime,
						token: Token
					},
					success: (res) => {
						if (res.statusCode == 404) {
							uni.showToast({
								icon: "none",
								title: "网页失踪了"
							})
						} else {
							if (res.data.infoAmount == 0) {
								uni.showToast({
									icon: "none",
									title: "没有更多评论了"
								})
							}
							if (res.data.infoAmount > 0) {
								var tmp1 = {
									cmtId: res.data.cmtId1,
									cmtAuthorName: res.data.cmtAuthorName1,
									cmtAuthorImg: res.data.cmtAuthorImg1,
									cmtTime: res.data.cmtTime1,
									cmtContent: res.data.cmtContent1,
									cmtLike: res.data.cmtLike1,
									cmtLikeState: res.data.cmtLikeState1
								}
								_this.commentList.push(tmp1);
							}
							if (res.data.infoAmount > 1) {
								var tmp2 = {
									cmtId: res.data.cmtId2,
									cmtAuthorName: res.data.cmtAuthorName2,
									cmtAuthorImg: res.data.cmtAuthorImg2,
									cmtTime: res.data.cmtTime2,
									cmtContent: res.data.cmtContent2,
									cmtLike: res.data.cmtLike2,
									cmtLikeState: res.data.cmtLikeState2
								}
								_this.commentList.push(tmp2);
							}
							if (res.data.infoAmount > 2) {
								var tmp3 = {
									cmtId: res.data.cmtId3,
									cmtAuthorName: res.data.cmtAuthorName3,
									cmtAuthorImg: res.data.cmtAuthorImg3,
									cmtTime: res.data.cmtTime3,
									cmtContent: res.data.cmtContent3,
									cmtLike: res.data.cmtLike3,
									cmtLikeState: res.data.cmtLikeState3
								}
								_this.commentList.push(tmp3);
							}
						}
					}
				})
			},

			onReachBottom() { //滑到底部
				let _this = this;
				let len = _this.commentList.length - 1;
				_this.getComment(len);
			}
		}
	}
</script>

<style>
	/* 文章内容界面 */
	.moodInfo {
		background-color: white;
		display: flex;
		flex-direction: row;
	}

	/* 头像 */
	.portrait {
		margin: 2% 2% 2% 5%;
	}

	.likeAndCmt {
		position: fixed;
		bottom: 0%;
		display: flex;
		flex-direction: row;
		font-size: 20px;
		border: 1px solid #efefef;
		width: 100%;
		height: 7%;
	}

	.cmt {
		display: flex;
		flex-direction: row;
		margin-left: 15%;
		margin-right: 35%;
	}

	.thumb {
		display: flex;
		flex-direction: row;
	}

	.cmtText {
		margin-top: 11%;
	}

	.thbText {
		margin-top: 11%;
	}

	.tags {
		display: flex;
		flex-direction: row;
		margin-top: 5%;
		margin-left: 3%;
	}

	.miniTag {
		margin: 1%;
	}

	.LikesAndDisl {
		margin-left: 92%;
	}
</style>
