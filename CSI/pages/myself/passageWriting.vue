<template>
	<view class="passageWriting">

		<!-- 编辑文章内容区域 -->
		<view class="writingArea">
			<u--input class="infoAge"  placeholder="请输入文章标题" border="surround" v-model="title">
			</u--input>
			<u--input class="infoAge" placeholder="请输入文章简介" border="surround" v-model="abstract">
			</u--input>
			<u--textarea placeholder="请输入内容" height=180 maxlength=512 adjustPosition count></u--textarea>
		</view>

		<!-- 试听区域 -->
		<view class="player">
			<audio style="text-align: left" :src="current.src" :poster="current.poster" :name="current.name"
				:author="current.author" :loop="true" :action="audioAction" controls @pause="pause()" @timeupdate="timeupdate()" @play="play()"></audio>
		</view>

		<!-- 声音设置区域 -->
		<view class="voiceChoice">
			<navigator url="/pages/set/voiceSet">
				<u-button type="primary" color="rgb(135,206,250)" text="声音设置"></u-button>
			</navigator>
		</view>

		<!-- 按钮区域 -->
		<view class="btn">

			<!--取消按钮-->
			<view class="btnQuit">
				<u-button type="primary" color="gray" text="取消" @click="quit()"></u-button>
			</view>

			<!--发表按钮-->
			<view class="btnPublish">
				<u-button type="primary" text="发表" @click="publish()"></u-button>
			</view>
		</view>



	</view>
</template>

<script>
	export default {
		data() {
			return {
				title: '标题',
				abstract: '',
				passageContent: '',
				current: {
					poster: 'https://bjetxgzv.cdn.bspapp.com/VKCEYUGU-uni-app-doc/7fbf26a0-4f4a-11eb-b680-7980c8a877b8.png',
					name: '文章标题',
					author: '简介',
					src: 'https://bjetxgzv.cdn.bspapp.com/VKCEYUGU-hello-uniapp/2cc220e0-c27a-11ea-9dfb-6da8e309e0d8.mp3',
					loop: 'true',
				},
				audioAction: {
					method: 'pause'
				},
			}
		},
		methods: {
			quit() {
				uni.navigateTo({
					url: './myselfMain'
				});
			},
			publish() {

			}
		}
	}
</script>

<style>
	.player{
		margin: 5% 2% 5% 2%;
		display: flex;
		flex-direction: row;
		justify-content: center;
		
	}
	
	.voiceChoice {
		margin: 5% 5% 50% 5%;

	}

	/* 按钮区域 */

	.btn {
		display: flex;
		flex-direction: row;
		margin-top: 10px;
		margin-right: 10px;
		margin-bottom: 10px;
		margin-left: 10px;
		align-items: center;
		justify-content: center;
	}

	/* 注册按钮 */
	.btnQuit {
		margin-top: 10px;
		margin-right: 30%;
		margin-bottom: 10px;
		margin-left: 10%;
		text-align: right;

	}

	/* 返回按钮 */
	.btnPublish {
		margin-top: 10px;
		margin-right: 10%;
		margin-bottom: 10px;
		margin-left: 30%;
		text-align: right;
	}
</style>
