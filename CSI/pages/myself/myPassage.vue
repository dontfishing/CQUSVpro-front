<template>
	<view class="passageGround">

		<!-- 具体内容 -->
		<view class="contentArea">
			<view v-for="(item, index) in passageList" :key="index">
				<uni-card :title="passageList[index].userName" :sub-title="passageList[index].time"
					@click="goToDetail(index)"
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
						<u-button class="deleteBtn" type="primary" size="mini" color="#f56c6c" @click="deletePass(index)"></u-button>
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
				passageList: [],
				loadMoreStatus: 'more', // 加载更多的状态：可加载、正在加载、没有更多

			}
		},
		methods: {
			play() { // 播放音频

			},
			updatePass(ob) {
				if (ob.infoAmount > 0) { //更新第一篇
					var tmp3 = {
						userName: "",
						time: "",
						title: "",
						abstract: "",
						likesSum: 0,
						commentSum: 0,
						poster: '',
						src: ''
					};
					tmp3.userName = ob.essayUserName3; //用户名
					tmp3.title = ob.essayTitle3; //文章标题
					tmp3.abstract = ob.essaySummary3; // 简介
					tmp3.time = ob.essayPostTime3; // 发布时间
					tmp3.likesSum = ob.essayPostLike3; // 点赞数
					tmp3.commentSum = ob.essayComment3; // 评论数
					tmp3.id = ob.essayPostId3; //文章id
					
					tmp3.poster =
						'https://bjetxgzv.cdn.bspapp.com/VKCEYUGU-uni-app-doc/7fbf26a0-4f4a-11eb-b680-7980c8a877b8.png'; // 播放背景图片,默认为用户头像
					tmp3.src =
						'https://bjetxgzv.cdn.bspapp.com/VKCEYUGU-hello-uniapp/2cc220e0-c27a-11ea-9dfb-6da8e309e0d8.mp3'; // 音频来源
					this.passageList.push(tmp3); //更新文章列表
				}
				if (ob.infoAmount > 1) { //更新第二篇
					var tmp2 = {
						userName: "",
						time: "",
						title: "",
						abstract: "",
						likesSum: 0,
						commentSum: 0,
						poster: '',
						src: ''
					};
					tmp2.userName = ob.essayUserName2; //用户名
					tmp2.title = ob.essayTitle2; //文章标题
					tmp2.abstract = ob.essaySummary2; // 简介
					tmp2.time = ob.essayPostTime2; // 发布时间
					tmp2.likesSum = ob.essayPostLike2; // 点赞数
					tmp2.commentSum = ob.essayComment2; // 评论数
					tmp2.id = ob.essayPostId2; //文章id
					tmp2.poster =
						'https://bjetxgzv.cdn.bspapp.com/VKCEYUGU-uni-app-doc/7fbf26a0-4f4a-11eb-b680-7980c8a877b8.png'; // 播放背景图片,默认为用户头像
					tmp2.src =
						'https://bjetxgzv.cdn.bspapp.com/VKCEYUGU-hello-uniapp/2cc220e0-c27a-11ea-9dfb-6da8e309e0d8.mp3'; // 音频来源
					this.passageList.push(tmp2); //更新文章列表
				}
				if (ob.infoAmount > 2) { //	更新第三篇
					var tmp1 = {
						userName: "",
						time: "",
						title: "",
						abstract: "",
						likesSum: 0,
						commentSum: 0,
						poster: '',
						src: ''
					};
					tmp1.userName = ob.essayUserName1; //用户名
					tmp1.title = ob.essayTitle1; //文章标题
					tmp1.abstract = ob.essaySummary1; // 简介
					tmp1.time = ob.essayPostTime1; // 发布时间
					tmp1.likesSum = ob.essayPostLike1; // 点赞数
					tmp1.commentSum = ob.essayComment1; // 评论数
					tmp1.id = ob.essayPostId1; //文章id
					tmp1.poster =
						'https://bjetxgzv.cdn.bspapp.com/VKCEYUGU-uni-app-doc/7fbf26a0-4f4a-11eb-b680-7980c8a877b8.png'; // 播放背景图片,默认为用户头像
					tmp1.src =
						'https://bjetxgzv.cdn.bspapp.com/VKCEYUGU-hello-uniapp/2cc220e0-c27a-11ea-9dfb-6da8e309e0d8.mp3'; // 音频来源
					this.passageList.push(tmp1); //更新文章列表
				}


			},
			refresh(ob) { //下拉加载更多的函数，会传入最下面文章的发表时间
				var Token;
				uni.getStorage({ //获取login的token
					key: 'login_token',
					success: function(res) {
						Token = res.data;
					}
				});
				console.log(Token);
				let _this = this;
				uni.request({ //获取远端数据
					url: 'http://106.14.62.110:8080/user/essays/refresh',
					method:'POST',
					data: {
						token: Token,
						time: ob.time
					},
					success: (res) => {
						console.log(res.data);
						_this.updatePass(res.data);
					}
				})
			},
			pullDownRefresh() { //上拉刷新的函数
				var Token;
				uni.getStorage({ //获取login的token
					key: 'login_token',
					success: function(res) {
						Token = res.data;
					}
				});
				console.log(Token);
				let _this = this;
				uni.request({
					url: 'http://106.14.62.110:8080/user/essays',
					method:"POST",
					data:{
						token: Token,
					},
					success: (res) => {
						console.log(res.data);
						_this.updatePass(res.data);
						if (res.data.infoAmount == 0) {
							un.showToast({
								title: '没有更多了QAQ',
								duration: 2000
							})
						} else {
							uni.showToast({
								title: '刷新成功',
								duration: 2000
							})
						}
					}
				})
			},
			goToDetail(index) { //加载文章详情页
				let tmp =this.passageList[index];
				uni.setStorage({
					key: 'passage_detail',
					data: tmp,
					success: function() {
						console.log(JSON.stringify(tmp));
					}
				});
				uni.navigateTo({
					url:'./passageDetails'
				})
			},
			onLoad() { //每次加载都会重新刷新
				let that = this;
				that.pullDownRefresh();
			},
			onPullDownRefresh() { // 下拉刷新
				this.passageList = [];
				this.pullDownRefresh();
			},

			onReachBottom() { // 上划加载
				var len = this.passageList.length;
				console.log(JSON.stringify(this.passageList[len - 1]));
				this.refresh(this.passageList[len - 1]);
			},
			
			deletePass(index) {
				uni.request({
					url:'http://106.14.62.110:8080/user/essays/delete',
					method:'POST',
					data:{
						postId: passageList[index].id
					}
				})
			}
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
	
	.deleteBtn {
		margin-left: 50%;
	}
</style>