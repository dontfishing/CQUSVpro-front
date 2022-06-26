<template>
	<view class="contentArea">
		<uni-card :title="userName" :sub-title="submitTime"
			thumbnail="https://vkceyugu.cdn.bspapp.com/VKCEYUGU-dc-site/460d46d0-4fcc-11eb-8ff1-d5dcf8779628.png">
			<!-- 语音播放 -->
			<view class="player">
				<audio style="text-align: left" :src="current.src" :poster="current.poster" :name="current.name"
					:author="current.author" :loop="true" :action="audioAction" controls @play="play()"></audio>
			</view>
		</uni-card>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				userName: "标题",
				submitTime: "时间",
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
		}
	}
</script>

<style>
</style>
