 <template>
	<view class="passageGround">

		<!-- 具体内容 -->
		<view class="contentArea">
			<view v-for="(item, index) in passageList" :key="index">
				<uni-card :title="passageList[index].commentId" :sub-title="passageList[index].time"
					@click="goToDetail(index)"
					thumbnail="https://vkceyugu.cdn.bspapp.com/VKCEYUGU-dc-site/460d46d0-4fcc-11eb-8ff1-d5dcf8779628.png">
					<!--对方文章标题-->
					<u-text v-text="passageList[index].postTitle"></u-text>
					<!--评论内容-->
					<u-text v-text="passageList[index].cmtContent"></u-text>
					<!-- 语音播放 -->
					<view class="player">
						<audio style="text-align: left" :src="passageList[index].src"
							:poster="passageList[index].poster" :name="passageList[index].postTitle"
							:author="passageList[index].postId" :loop="false" controls @play="play()"></audio>
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
					<!--删除-->
					<u-button class="deleteBtn" type="primary" size="mini" color="#f56c6c" @click="deletePass(index)" text="删除"></u-button>
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
					var tmp1 = {
						commentId: 0, //评论ID
						time: "", //发布时间
						likesSum: 0, //点赞数量
						dislikesSum: 0, //点踩数量
						cmtContent: "", //评论内容
						src: '', //评论音频
						postId: 0, //评论对应文章的ID
						postTitle: "", //评论对应文章的标题
						poster: '',
						
					};
					tmp1.commentId = ob.cmtId1; //评论ID
					tmp1.time = ob.cmtTime1; //发布时间
					tmp1.likesSum = ob.cmtLike1; // 点赞数量
					tmp1.dislikesSum = ob.cmtDislike1; // 点踩数量
					tmp1.cmtContent = ob.cmtContent1; // 评论内容
					tmp1.src = ob.cmtTts1; // 评论音频
					tmp1.postId = ob.postId1; //评论对应文章的ID
					tmp1.postTitle =ob.postTitle1; //评论对应文章的标题
					tmp1.poster = 'https://bjetxgzv.cdn.bspapp.com/VKCEYUGU-uni-app-doc/7fbf26a0-4f4a-11eb-b680-7980c8a877b8.png';
					this.passageList.push(tmp1); //更新文章列表
					console.log(tmp1.commentId);
				}
				if (ob.infoAmount > 1) { //更新第二篇
					var tmp2 = {
						commentId: 0, //评论ID
						time: "", //发布时间
						likesSum: 0, //点赞数量
						dislikesSum: 0, //点踩数量
						cmtContent: "", //评论内容
						src: '', //评论音频
						postId: 0, //评论对应文章的ID
						postTitle: "", //评论对应文章的标题
						poster: ''
					};
					tmp2.commentId = ob.cmtId2; //评论ID
					tmp2.time = ob.cmtTime2; //发布时间
					tmp2.likesSum = ob.cmtLike2; // 点赞数量
					tmp2.dislikesSum = ob.cmtDislike2; // 点踩数量
					tmp2.cmtContent = ob.cmtContent2; // 评论内容
					tmp2.src = ob.cmtTts2; // 评论音频
					tmp2.postId = ob.postId2; //评论对应文章的ID
					tmp2.postTitle =ob.postTitle2; //评论对应文章的标题
					tmp2.poster = 'https://bjetxgzv.cdn.bspapp.com/VKCEYUGU-uni-app-doc/7fbf26a0-4f4a-11eb-b680-7980c8a877b8.png';
					this.passageList.push(tmp2); //更新文章列表
				}
				if (ob.infoAmount > 2) { //	更新第三篇
					var tmp3 = {
						commentId: 0, //评论ID
						time: "", //发布时间
						likesSum: 0, //点赞数量
						dislikesSum: 0, //点踩数量
						cmtContent: "", //评论内容
						src: '', //评论音频
						postId: 0, //评论对应文章的ID
						postTitle: "", //评论对应文章的标题
						poster: ''
					};
					tmp3.commentId = ob.cmtId3; //评论ID
					tmp3.time = ob.cmtTime3; //发布时间
					tmp3.likesSum = ob.cmtLike3; // 点赞数量
					tmp3.dislikesSum = ob.cmtDislike3; // 点踩数量
					tmp3.cmtContent = ob.cmtContent3; // 评论内容
					tmp3.src = ob.cmtTts3; // 评论音频
					tmp3.postId = ob.postId3; //评论对应文章的ID
					tmp3.postTitle =ob.postTitle3; //评论对应文章的标题
					tmp3.poster = 'https://bjetxgzv.cdn.bspapp.com/VKCEYUGU-uni-app-doc/7fbf26a0-4f4a-11eb-b680-7980c8a877b8.png';
					this.passageList.push(tmp3); //更新文章列表
				}


			},
			refresh(ob) { //上划加载更多的函数，会传入最下面文章的id
				var Token;
				uni.getStorage({ //获取login的token
					key: 'login_token',
					success: function(res) {
						Token = res.data;
						console.log('获取token success');
						console.log(res.data);
					},
					fail: function() {
						console.log('获取token fail');
					}
				});
				let _this = this;
				
				uni.request({ //获取远端数据
					url: 'http://106.14.62.110:8080/user/comment/refresh',
					method: "POST",
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
			pullDownRefresh() { //下拉刷新的函数
				var Token;
				uni.getStorage({ //获取login的token
					key: 'login_token',
					success: function(res) {
						Token = res.data;
						console.log('获取token success');
						console.log(res.data);
					},
					fail: function() {
						console.log('获取token fail');
					}
				});
				let _this = this;
				uni.request({
					url: 'http://106.14.62.110:8080/user/comments',
					method: 'POST',
					data:{
						token: Token
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
				let tmp = this.passageList[index].postId;
				uni.setStorage({ //存入缓存
					key: 'postID',
					data: tmp,
					success: function() {
						console.log(JSON.stringify(tmp));
					}
				});
				uni.navigateTo({
					url: './passageDetails'
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
					url:'http://106.14.62.110:8080/comment/delete',
					method:'POST',
					data:{
						cmtId: this.passageList[index].commentId
					},
					
					success: res => {
						if(res.statusCode == 404) {
							uni.showToast({
								icon:'error',
								title:'404 not found'
							})
						} else if("Info" in res.data && res.data[Info] == "delete failed") {
							uni.showToast({
								icon:'error',
								title:'删除失败'
							})		
						} else {
							uni.showToast({
								icon:'success',
								title:'删除成功'
							})
						}
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
		margin: 0px 6% 0px 5%;

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
		margin-left: 60%;
	}
</style>