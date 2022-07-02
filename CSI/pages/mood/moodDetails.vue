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
			<u--text :text="moodContent" margin="1% 1% 1% 1%"></u--text>
		</view>
		<!-- 图片 -->
		<view>
			<image :src="imgSrc" mode="aspectFit"></image>
		</view>
		<!--tag部分-->
		<view class="tags">
			<u-tag class="miniTag" v-for="(item, index) in tagList" :key="index" :text="tagList[index]"
			size="mini" plain borderColor="#909399" color="#909399" shape="circle"></u-tag>
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
				<text  class="cmtText">评论</text>
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
			return{
				portraitSrc: 'https://bjetxgzv.cdn.bspapp.com/VKCEYUGU-uni-app-doc/7fbf26a0-4f4a-11eb-b680-7980c8a877b8.png',
				userName: '用户名',
				submitTime: '2022-7-2',
				moodContent: '乐死我了',
				imgSrc: 'https://bjetxgzv.cdn.bspapp.com/VKCEYUGU-uni-app-doc/7fbf26a0-4f4a-11eb-b680-7980c8a877b8.png',
				tagList: ["乐死了","马脸","颜值：12.33","年龄：20","高兴"], //标签列表
				commentList: [{
					cmtAuthorName: "哈哈哈",
					cmtTime: "2022-7-2",
					cmtAuthorImg: "https://bjetxgzv.cdn.bspapp.com/VKCEYUGU-uni-app-doc/7fbf26a0-4f4a-11eb-b680-7980c8a877b8.png",
					cmtContent: "123",
					cmtLikeState: 0
				}],
				likeIcon: "thumb-up", //点赞图标的状态，默认是未点赞的
				likeSum: 0 //点赞数
			}
		},
		
		onLoad: function() { //进入页面时加载
			let that = this;
			that.getMoodInfo();
		},
		
		methods: {
			comment() { //点击评论，进入写评论界面
				uni.navigateTo({
					url:'/pages/mood/moodCmtWrite'
				})
			},
			
			thumb() { //心情点赞
				if (this.likeIcon == "thumb-up-fill") { //若是已经点赞了,就取消点赞
					uni.request({
						url: '', //api地址
						method: "POST",
						data: {

						},
						success: res => {
							if (res.statusCode == 404) { //返回的状态码
								uni.showToast({
									icon: 'error',
									title: '网页失踪了',
								});
							}else if('erro'in res.data){
								uni.showToast({
									title: 'error'
								})
							}
							 else {
								this.likeIcon = "thumb-up";
								this.likeSum--;
							}
						},
						fail: () => {

						}
					});
				} else { //点赞
					uni.request({
						url: '', //api地址
						method: "POST",
						data: {

						},
						success: res => {
							console.log("点赞");
							console.log(res.data);

							if (res.statusCode == 404) { //返回的状态码
								uni.showToast({
									icon: 'error',
									title: '网页失踪了',
								});
							}else if('erro'in res.data){
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
				
			},
			
			clickComLikeIcon(index) { // 点击评论点赞按钮
				if (this.commentList[index].cmtLikeState) { // 取消点赞
					uni.request({
						url: '', //api地址
						method: "POST",
						data: {
							
						},
						success: res => {
							console.log(res.data);
							if (res.statusCode == 404) { //返回的状态码
								uni.showToast({
									icon: 'error',
									title: '网页失踪了',
								});
							}else if('erro'in res.data){
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
						url: '', //api地址
						method: "POST",
						data: {
							cmtId: this.commentList[index]["cmtId"],
							token:this.token
						},
						success: res => {
							console.log(res.data);
							if (res.statusCode == 404) { //返回的状态码
								uni.showToast({
									icon: 'error',
									title: '网页失踪了',
								});
							}else if('erro'in res.data){
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
				}
			},
			
			getComment() {
				
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
	}
	
	.miniTag {
		margin: 1%;
	}
	
	.passageContent {
		font-size: 20px;
	}
	
	.LikesAndDisl {
		margin-left: 92%;
	}
</style>