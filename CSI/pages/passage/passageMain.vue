<template>
	<view class="passageGround">

		<!-- 搜索区域 -->
		<view class="searchArea">
			<!-- 搜索栏 -->
			<view class="searchBar">
				<u-search placeholder="请输入搜索关键词" borderColor="#e9e9e9" shape="square" v-model="keyword" clearabled
					bgColor="white"></u-search>
			</view>
		</view>

		<!-- 具体内容 -->
		<view class="contentArea">
			<view v-for="(item, index) in passageList" :key="index">
				<uni-card :title="passageList[index].userName" :sub-title="passageList[index].time"
					thumbnail="https://vkceyugu.cdn.bspapp.com/VKCEYUGU-dc-site/460d46d0-4fcc-11eb-8ff1-d5dcf8779628.png">
					<!-- 语音播放 -->
					<view class="player">
						<audio style="text-align: left" :src="passageList[index].src"
							:poster="passageList[index].poster" :name="passageList[index].title"
							:author="passageList[index].abstract" :loop="false" controls @play="play()"></audio>
					</view>
					<!-- 点赞评论栏 -->
					<view class="comAndLikes">
						<!-- 评论 -->
						<view class="Comment">
							<u-icon name="chat" size="25"></u-icon>
							<!-- 评论数 -->
							<u--text v-text="passageList[index].commentSum"></u--text>
						</view>
						<!-- 点赞 -->
						<view class="Likes">
							<u-icon name="thumb-up" size="25"></u-icon>
							<!-- 点赞数 -->
							<u--text v-text="passageList[index].likesSum"></u--text>
						</view>
					</view>
				</uni-card>
			</view>
		</view>

		<!-- 加载页 -->
		<view class="loadmore">
			<uni-load-more :status="loadMoreStatus"></uni-load-more>
		</view>
	</view>
</template>
<script>
	import uniLoadMore from '@/uni_modules/uni-load-more/components/uni-load-more/uni-load-more.vue'

	export default {
		data() {
			return {
				keyword: "", // 搜索关键词
				passageList: [{ // 文章列表
					passId: "1",
					userName: "123", //用户名
					time: "2022.1.3 1:10", // 发布时间
					title: "1", // 标题
					abstract: "1", // 简介
					likesSum: 55, // 点赞数
					commentSum: 10, // 评论数
					poster: 'https://bjetxgzv.cdn.bspapp.com/VKCEYUGU-uni-app-doc/7fbf26a0-4f4a-11eb-b680-7980c8a877b8.png', // 播放背景图片,默认为用户头像
					src: 'https://bjetxgzv.cdn.bspapp.com/VKCEYUGU-hello-uniapp/2cc220e0-c27a-11ea-9dfb-6da8e309e0d8.mp3', // 音频来源
				}, {
					// 同上
					passId: "1",
					userName: "345", //用户名
					title: "2", // 标题
					abstract: "2", // 简介
					time: "2022.1.3 1:10", // 发布时间
					likesSum: 25, // 点赞数
					commentSum: 40, // 评论数
					poster: 'https://bjetxgzv.cdn.bspapp.com/VKCEYUGU-uni-app-doc/7fbf26a0-4f4a-11eb-b680-7980c8a877b8.png', // 播放背景图片,默认为用户头像
					src: 'https://bjetxgzv.cdn.bspapp.com/VKCEYUGU-hello-uniapp/2cc220e0-c27a-11ea-9dfb-6da8e309e0d8.mp3', // 音频来源
				}],
				loadMoreStatus: 'more', // 加载更多的状态：可加载、正在加载、没有更多

			}
		},
		methods: {
			play() { // 播放音频

			},
			updatePass(ob) {
				var tmp;
				tmp.userName = ob.userName; //用户名
				tmp.title = ob.postTitlem; //文章标题
				tmp.abstract = ob.postSummary; // 简介
				tmp.time = ob.postTime; // 发布时间
				tmp.likesSum = ob.postLike; // 点赞数
				tmp.commentSum = ob.postComment; // 评论数
				tmp.id = ob.postId; //文章id
				tmp.poster =
					'https://bjetxgzv.cdn.bspapp.com/VKCEYUGU-uni-app-doc/7fbf26a0-4f4a-11eb-b680-7980c8a877b8.png'; // 播放背景图片,默认为用户头像
				tmp.src =
					'https://bjetxgzv.cdn.bspapp.com/VKCEYUGU-hello-uniapp/2cc220e0-c27a-11ea-9dfb-6da8e309e0d8.mp3'; // 音频来源
				this.passageList.push(tmp); //更新文章列表
			},
			refresh(ob) { //下拉加载更多的函数
				let _this = this;
				uni.request({	//获取远端数据
					url: 'http://106.14.62.110:8080/essay/afterRefresh',
					data: {
						postId: ob.postId
					},
					success: (res) => {
						console.log(res.data);
						if (res.data.infoAmount == 0) {
							uni.showToast({
								title: '刷新成功',
								duration: 2000
							});
						} else if (res.data.infoAmount == 1) {
							_this.updatePass(res.data.essay1);
							uni.showToast({
								title: '刷新成功',
								duration: 2000
							});
						} else if (res.data.infoAmount == 2) {
							_this.updatePass(res.data.essay1);
							_this.updatePass(res.data.essay2);
							uni.showToast({
								title: '刷新成功',
								duration: 2000
							});
						} else {
							_this.updatePass(res.data.essay1);
							_this.updatePass(res.data.essay2);
							_this.updatePass(res.data.essay3);
							uni.showToast({
								title: '刷新成功',
								duration: 2000
							});
						}
					}
				})
			},
			pullDownRefresh() { //上拉刷新的函数
				let _this = this;
				uni.request({
					url: 'http://106.14.62.110:8080/essay/refresh',
					success: (res) => {
						console.log(res.data);
						if (res.data.infoAmount == 0) {
							uni.showToast({
								title: '刷新成功',
								duration: 2000
							});
						} else if (res.data.infoAmount == 1) {
							_this.updatePass(res.data.essay1);
							uni.showToast({
								title: '刷新成功',
								duration: 2000
							});
						} else if (res.data.infoAmount == 2) {
							_this.updatePass(res.data.essay1);
							_this.updatePass(res.data.essay2);
							uni.showToast({
								title: '刷新成功',
								duration: 2000
							});
						} else {
							_this.updatePass(res.data.essay1);
							_this.updatePass(res.data.essay2);
							_this.updatePass(res.data.essay3);
							uni.showToast({
								title: '刷新成功',
								duration: 2000
							});
						}
					}
				})
			},
			onLoad() { //每次加载都会重新刷新
				let that = this;
				that.pullDownRefresh();
			},
			onPullDownRefresh() { // 下拉刷新
				this.pullDownRefresh();
			},

			onReachBottom() { // 上划加载
				this.refresh(this.passageList[this.passageList.length - 1]);
			},
		}
	}
</script>
<style>
	.passageGround {}

	.searchArea {
		background-color: #e9e9e9;
	}

	.searchBar {
		margin: 5px 10px 20px 5px;
	}

	.contentArea {}

	.player {
		display: flex;
		flex-direction: row;
	}

	/* 	.progressLine{
		margin: 2px 2px 0px 0px;
		
		justify-content: center;
	}
 */
	.comAndLikes {
		display: flex;
		flex-direction: row;
		margin: 2px 10% 2px 2px;
	}

	.Comment {
		margin: 0px 80% 0px 5%;

	}

	.loadmore {
		display: flexbox;
		flex-direction: column-reverse;
		margin-top: 20%;
	}

	.Likes {
		//点赞数
		display: flex;
	}

	.Comment {
		//评论数
		display: flex;
	}
</style>
