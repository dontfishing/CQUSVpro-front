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
				<uni-card title="'title" sub-title="简介" :extra="getAcAndTime()"
					thumbnail="https://vkceyugu.cdn.bspapp.com/VKCEYUGU-dc-site/460d46d0-4fcc-11eb-8ff1-d5dcf8779628.png">
					<!-- 语音播放条 -->
					<view class="player">
						<!-- 播放图标 -->
						<u-icon name="play-circle-fill" size="25" @click="play()"></u-icon>
						<!-- 播放进度信息 -->
						<view class="progressInfo">
							<text>0/1</text>
						</view>
						<!-- 进度条 -->
						<view class="progressLine">
							<u-line-progress :percentage="30" activeColor="gray" :showText="false" height="8">
							</u-line-progress>
						</view>
						<u-icon name="volume" size="25"></u-icon>
					</view>
					<!-- 点赞评论栏 -->
					<view class="comAndLikes">
						<!-- 评论 -->
						<view class="Comment">
							<u-icon name="chat" size="25"></u-icon>
							<u--text v-text="commentSum"></u--text>
						</view>
						<!-- 点赞 -->
						<view class="Likes">
							<u-icon name="thumb-up" size="25"></u-icon>
							<u--text v-text="likesSum"></u--text>
						</view>
					</view>
				</uni-card>
			</view>
		</view>

	</view>
</template>
<script>
	export default {
		data() {
			return {
				keyword: "",

				passageList: [{
					title: "1",
					resume: "1",
					account: "lsw",
					time: "2022.3.3",
					likesSum: 55,
					commentSum: 10
				}, {
					title: "2",
					resume: "2",
					account: "smc",
					time: "2022.4.5",
					likesSum: 40,
					commentSum: 5
				}],
			}
		},
		methods: {
			getAcAndTime() {
				// 	var data = {
				// 		account: this.passageList.account,
				// 		time: this.passageList.time
				// 	};
				// 	return valueAc+"\n"+time;
			},

		},
	}
</script>
<style>
	.passageGround {}

	.searchArea {
		background-color: #e9e9e9;
	}

	.searchBar {
		margin: 5px 10px 10px 5px;
	}

	.contentArea {}

	.player {
		display: flex;
		flex-direction: row;
		background-color: gainsboro;
	}

	/* 	.progressLine{
		margin: 2px 2px 0px 0px;
		
		justify-content: center;
	}
 */
	.comAndLikes {
		display: flex;
		flex-direction: row;
		margin: 2px 2px 2px 2px;
	}

	.Comment {
		margin: 0px 30% 0px 0px;

	}
</style>
