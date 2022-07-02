<template>
	<view class="passageGround">

		<!-- 具体内容 -->
		<view class="contentArea">
			<view v-for="(item, index) in moodList" :key="index">
				<uni-card :title="moodList[index].moodAuthorName" :sub-title="moodList[index].moodTime"
					@click="goToDetail(index)" :thumbnail="moodList[index].moodAuthorImg">

					<!-- 心情主题 -->
					<view class="moodTheme">
						<u--text :text="moodList[index].moodContent"></u--text>
					</view>

					<!-- 图片 -->
					<view class="sharedImg">
						<u--image :src="moodList[index].moodImg" width="80px" height="80px"></u--image>
					</view>

					<!-- 标签 -->
					<view class="labelArea">
						<view v-for="(i, j) in moodList[index].moodTag" :key="j">
							<u-tag class="eachLabel" :text="moodList[index].moodTag[j]" plain shape="circle"
								color="#000000" borderColor="#e5e5e5"></u-tag>
						</view>
					</view>

					<!-- 点赞评论栏 -->
					<view class="comAndLikes">
						<!-- 评论 -->
						<view class="Comment">
							<u-icon name="chat" size="25"></u-icon>
							<!-- 评论数 -->
							<text>{{moodList[index].moodComment}}</text>
							<!-- <u--text :v-text="moodList[index].commentSum"></u--text> -->
						</view>
						<!-- 点赞 -->
						<view class="Likes">
							<u-icon name="thumb-up" size="25"></u-icon>
							<!-- 点赞数 -->
							<text>{{moodList[index].moodLike}}</text>
							<!-- <u--text :v-text="moodList[index].likesSum"></u--text> -->
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
				moodList: [],
				loadMoreStatus: 'more', // 加载更多的状态：可加载、正在加载、没有更多

			}
		},
		methods: {
			play() { // 播放音频
			},
			updateMood(ob) {
				if (ob.infoAmount <3) {
					this.loadMoreStatus = 'noMore';
				}
				if (ob.infoAmount > 0) { //更新第一篇
					var tmp1 = {
						moodAuthorName: "",
						moodAuthorImg: "",
						moodContent: "",
						moodTag: [],
						moodTime: "",
						moodLike: 0,
						moodComment: 0,
						moodImg: '',
						moodId: 0
					};
					tmp1.moodAuthorName = ob.moodAuthorName1; //用户名
					tmp1.moodAuthorImg = ob.moodAuthorImg1; //头像
					tmp1.moodContent = ob.moodContent1; // 心情主题
					//标签
					if(ob.moodTag1.moodTagMood=="life"){
						tmp1.moodTag.push("life");
					}
					else{
						tmp1.moodTag.push(ob.moodTag1.moodTagAge);
						tmp1.moodTag.push(ob.moodTag1.moodTagLevel);
						tmp1.moodTag.push(ob.moodTag1.moodDetailMood);
						tmp1.moodTag.push(ob.moodTag1.moodTagShape);
					}
					tmp1.moodTime = ob.moodTime1; // 发布时间
					tmp1.moodImg = ob.moodImg1; //心情图片
					tmp1.moodLike = ob.moodLike1; // 点赞数
					tmp1.moodComment = ob.moodComment1; // 评论数
					tmp1.moodId = ob.moodId1; //心情id
					this.moodList.push(tmp1); //更新心情列表
				}
				if (ob.infoAmount > 1) { //更新第二篇
					var tmp2 = {
						moodAuthorName: "",
						moodAuthorImg: "",
						moodContent: "",
						moodTag: [],
						moodTime: "",
						moodLike: 0,
						moodComment: 0,
						moodImg: '',
						moodId: 0
					};
					tmp2.moodAuthorName = ob.moodAuthorName2; //用户名
					tmp2.moodAuthorImg = ob.moodAuthorImg2; //头像
					tmp2.moodContent = ob.moodContent2; // 心情主题
					if(ob.moodTag2.moodTagMood=="life"){
						tmp2.moodTag.push("life");
					}
					else{
						tmp2.moodTag.push(ob.moodTag2.moodTagAge);
						tmp2.moodTag.push(ob.moodTag2.moodTagLevel);
						tmp2.moodTag.push(ob.moodTag2.moodDetailMood);
						tmp2.moodTag.push(ob.moodTag2.moodTagShape);
					}
					tmp2.moodTime = ob.moodTime2; // 发布时间
					tmp2.moodImg = ob.moodImg2; //心情图片
					tmp2.moodLike = ob.moodLike2; // 点赞数
					tmp2.moodComment = ob.moodComment2; // 评论数
					tmp2.moodId = ob.moodId2; //心情id
					this.moodList.push(tmp2); //更新心情列表					console.log(1);

				}
				if (ob.infoAmount > 2) { //	更新第三篇
					var tmp3 = {
						moodAuthorName: "",
						moodAuthorImg: "",
						moodContent: "",
						moodTag: [],
						moodTime: "",
						moodLike: 0,
						moodComment: 0,
						moodImg: '',
						moodId: 0
					};
					tmp3.moodAuthorName = ob.moodAuthorName3; //用户名
					tmp3.moodAuthorImg = ob.moodAuthorImg3; //头像
					tmp3.moodContent = ob.moodContent3; // 心情主题
					if(ob.moodTag3.moodTagMood=="life"){
						tmp3.moodTag.push("life");
					}
					else{
						tmp3.moodTag.push(ob.moodTag3.moodTagAge);
						tmp3.moodTag.push(ob.moodTag3.moodTagLevel);
						tmp3.moodTag.push(ob.moodTag3.moodDetailMood);
						tmp3.moodTag.push(ob.moodTag3.moodTagShape);
					}
					tmp3.moodTime = ob.moodTime3; // 发布时间
					tmp3.moodImg = ob.moodImg3; //心情图片
					tmp3.moodLike = ob.moodLike3; // 点赞数
					tmp3.moodComment = ob.moodComment3; // 评论数
					tmp3.moodId = ob.moodId3; //心情id
					this.moodList.push(tmp3); //更新心情列表
				}


			},
			refresh(ob) { //上划加载更多的函数，会传入最下面心情的id
				let _this = this;
				let selectedList = uni.getStorageSync('selectedList');
				let ageList = selectedList.age;
				let moodList = selectedList.mood;
				let shapeList = selectedList.shape;

				const moodId = ob.moodId;

				uni.request({ //获取远端数据
					url: 'http://106.14.62.110:8080/feeling/refresh',
					method: "POST",
					data: {
						ageTag: ageList,
						faceTag: shapeList,
						moodTag: moodList,
						moodId: moodId
					},
					success: (res) => {
						console.log("上划",res.data);
						_this.updateMood(res.data);
					}
				})
			},
			pullDownRefresh() { //下拉刷新的函数
				let selectedList = uni.getStorageSync('selectedList');
				
				let ageList = selectedList.age;
				let moodList = selectedList.mood;
				let shapeList = selectedList.shape;
				let _this = this;
				uni.request({
					url: 'http://106.14.62.110:8080/feeling',
					method: "POST",
					data: {
						ageTag: ageList,
						faceTag: shapeList,
						moodTag: moodList
					},
					success: (res) => {
						console.log("下拉:",res.data);
						_this.updateMood(res.data);
						if (res.data.infoAmount == 0) {
							uni.showToast({
								title: '没有更多了QAQ',
								icon:'fail',
								duration: 2000
							})
						} else {
							uni.showToast({
								title: '刷新成功',
								duration: 2000
							});
							uni.stopPullDownRefresh();
						}
					}
				})
			},
			goToDetail(index) { //加载心情详情页
				let tmp = this.moodList[index].moodId;
				uni.setStorage({ //存入缓存
					key: 'moodId',
					data: tmp,
					success: function() {}
				});

				uni.navigateTo({
					url: 'moodDetails'
				})
			},
			onLoad() { //每次加载都会重新刷新
				let that = this;
				that.pullDownRefresh();
			},
			onPullDownRefresh() { // 下拉刷新
				this.moodList = [];
				this.pullDownRefresh();
			},

			onReachBottom() { // 上划加载
				var len = this.moodList.length;
				// console.log(JSON.stringify(this.moodList[len - 1]));
				this.refresh(this.moodList[len - 1]);
			},
		}
	}
</script>
<style>
	.contentArea {
		flex-direction: column;
	}
	
	.labelArea{
		margin: 2% 2% 2% 2%;
		flex-wrap: wrap;
		display: flex;
		flex-direction: row;
	}
	
	.eachLabel{
		margin: 2rpx 2rpx 2rpx 2rpx;
		display: flex;
		flex-direction: row;
		
	}

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
		margin-top: 10%;
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
