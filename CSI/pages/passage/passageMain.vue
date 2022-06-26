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
				<uni-card title="用户名" sub-title="时间" :extra="getAcAndTime()"
					thumbnail="https://vkceyugu.cdn.bspapp.com/VKCEYUGU-dc-site/460d46d0-4fcc-11eb-8ff1-d5dcf8779628.png">
					<!-- 语音播放 -->
					<view class="player">
						<audio style="text-align: left" :src="current.src" :poster="current.poster" :name="current.name"
							:author="current.author" :loop="true" controls @play="play()"></audio>
					</view>
					<!-- 点赞评论栏 -->
					<view class="comAndLikes">
						<!-- 评论 -->
						<view class="Comment">
							<u-icon name="chat" size="25"></u-icon>
							<!-- 评论数 -->
							<u--text v-text="passageList.commentSum"></u--text>
						</view>
						<!-- 点赞 -->
						<view class="Likes">
							<u-icon name="thumb-up" size="25"></u-icon>
							<!-- 点赞数 -->
							<u--text v-text="passageList.likesSum"></u--text>
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
				// 搜索关键词
				keyword: "",
				// 当前播放状态
				current: {
					// 播放背景图片
					poster: 'https://bjetxgzv.cdn.bspapp.com/VKCEYUGU-uni-app-doc/7fbf26a0-4f4a-11eb-b680-7980c8a877b8.png',
					// 
					name: '文章标题',
					// author为自带命名，直接忽略，这个位置放的是简介
					author: '简介',
					// 音频来源
					src: 'https://bjetxgzv.cdn.bspapp.com/VKCEYUGU-hello-uniapp/2cc220e0-c27a-11ea-9dfb-6da8e309e0d8.mp3',
					// 是否循环播放
					loop: 'true',
				},
				
				// 文章列表
				passageList: [{
					// 标题
					title: "1",  
					// 简介
					resume: "1",
					// 用户名或昵称
					account: "lsw",
					// 发布时间
					time: "2022.3.3",
					// 点赞数
					likesSum: 55,
					// 评论数
					commentSum: 10
				}, {
					// 同上
					title: "2",
					resume: "2",
					account: "smc",
					time: "2022.4.5",
					likesSum: 40,
					commentSum: 5
				}],
				// 加载更多的状态：可加载、正在加载、没有更多
				loadMoreStatus: 'more',

			}
		},
		methods: {
			// 获取用户名和时间
			getAcAndTime() {
				// 	var data = {
				// 		account: this.passageList.account,
				// 		time: this.passageList.time
				// 	};
				// 	return valueAc+"\n"+time;
			},
			// 播放音频
			play() {

			},
			// loadmore(){
			// 	this.loadMoreStatus='loading';
			// }
			
		},
		
		onLoad: function(options) {
			setTimeout(function() {
				console.log('start pulldown');
			}, 1000);
			uni.startPullDownRefresh();
		},
		
		// 下拉刷新
		onPullDownRefresh() {
			console.log('refresh');
			setTimeout(function() {
				uni.stopPullDownRefresh();
			}, 1000);
		},
		
		// 上划加载
		onReachBottom() {
			let _self = this
			this.status = 'loading'
			uni.showNavigationBarLoading()
			this.pageNumber++
			uni.request({
				url: "",
				method: 'GET',
				header: {
					'content-type': 'application/json',
					'appCode': getApp().globalData.appCode
				},
				data: {
					"pageNumber":this.pageNumber,
					"pageSize":this.pageSize
				},
				success: (res) => {
					uni.hideLoading();
					const state = res.data.code;
					if (state == 0) {
						if (res.data) {
							this.upLoadData = res.data.data;
							// console.log(this.upLoadData.length)
							if(Math.ceil(res.data.total/this.pageSize) +1 <= this.pageNumber){
								_self.status = 'noMore'
							}else{
								setTimeout(function() {
									for (var i = 0; i < _self.upLoadData.length; i++) {
										_self.listData.push(_self.upLoadData[i])
									}
									console.log(_self.listData.length)
									_self.status = 'more'
									uni.hideNavigationBarLoading()
								}, 1000);
							}
						} else {
							this.showMore = false;
						}
					} else {
						uni.showToast({
							icon: 'none',
							title: res.data.msg
						});
					}
				}
			});
		},
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
</style>
