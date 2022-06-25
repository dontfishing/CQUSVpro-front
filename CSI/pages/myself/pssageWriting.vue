<template>
	<view class="passageWriting">

		<!-- 编辑文章内容区域 -->
		<view class="writingArea">

		</view>

		<!-- 试听区域 -->
		<view class="player">
			<audio style="text-align: left" :src="current.src" :poster="current.poster" :name="current.name"
				:author="current.author" :loop="true" :action="audioAction" controls @play="play()"></audio>
		</view>
		
		<!-- 按钮区域 -->
		<view class="btn">
			
			<!--取消按钮-->
			<view class="btnQuit">
				<navigator url="/pages/login/login">
					<u-button type="primary" text="取消"></u-button>
				</navigator>
			</view>
			
			<!--发表按钮-->
			<view class="btnPublish">
				<u-button type="primary" color="#" text="发表" @click="publish()"></u-button>
			</view>
		</view>

		<!-- 音色选择区域 -->
		<view class="voiceChoice">

		</view>

	</view>
</template>

<script>
	export default {
		data() {
			return {
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
			},
		methods:{
			
		},
	}
</script>

<style>
</style>
