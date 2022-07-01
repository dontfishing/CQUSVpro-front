<template>
	<view class="passageWriting">
		<!-- 编辑文章内容区域 -->
		<view class="writingArea">
			<u--input class="infoAge" placeholder="请输入文章标题" border="surround" maxlength=12 v-model="title">
			</u--input>
			<u--input class="infoAge" placeholder="请输入文章简介" border="surround" maxlength=15 v-model="abstract">
			</u--input>
			<u--textarea v-model="passageContent" placeholder="请输入内容" height=180 maxlength=512 :count="true"
				confirmType="done">
			</u--textarea>
		</view>
		<!-- 试听区域 -->
		<view class="player" v-show="show">
			<audio style="text-align: left" :src="current.src" :poster="current.poster" :name="title" :author="abstract"
				controls :action="audioAction" @pause="pause()" @play="play()"></audio>
		</view>
		<!--选择显示还是不显示-->
		<view class="voiceGenerate">
			<u-button text="生成音频" color="#8967D3" @click="generate()"></u-button>
		</view>
		<!-- 声音设置区域 -->
		<view class="voiceChoice">
			<navigator url="/pages/set/voiceSet">
				<u-button type="primary" color="#8967D3" text="声音设置"></u-button>
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
				<u-button type="primary" text="发表" @click="publish()" color="#8967D3"></u-button>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				title: '', //文章标题
				abstract: '', //文章简介
				passageContent: '', //文章内容
				current: { //提供播放器的标题，作者，资源
					poster: 'https://bjetxgzv.cdn.bspapp.com/VKCEYUGU-uni-app-doc/7fbf26a0-4f4a-11eb-b680-7980c8a877b8.png',
					src: '',
				},
				voiceSetting: { //语音设置
					ttsSpd: 5,
					ttsPit: 5,
					ttsVol: 5,
					ttsPer: 5
				},
				audioAction: {
					method: 'pause' //默认暂停
				},
				show: false //播放器默认不显示
			}
		},
		methods: {
			quit() { //取消后跳回与我相关界面
				uni.switchTab({
					url: './myselfMain'
				})
			},
			getVoiceSetting() {
				let _this = this;
				_this.voiceSetting = uni.getStorageSync('voice_setting');
			},
			generate() { //生成音频
				this.getVoiceSetting();
				let _this=this;
				if (this.passageContent == "") {
					uni.showToast({
						icon: "error",
						title: '文章内容不能为空!'
					});
				} else {
					let passPer = this.voiceSetting.ttsPer;
					let passSpd = this.voiceSetting.ttsSpd;
					let passPit = this.voiceSetting.ttsPit;
					let passVol = this.voiceSetting.ttsVol;
					let passContent = this.passageContent;
					uni.request({ //请求音频
						url: 'http://106.14.62.110:8080/sound/generate',
						method: "POST",
						data: {
							ttsPer: passPer,
							ttsSpd: passSpd,
							ttsPit: passPit,
							ttsVol: passVol,
							postContent: passContent
						},
						success: (res) => {
							console.log(res.data);
							if("postTts" in res.data && res.data.postTts !="") {
								this.current.src = res.data.postTts;
								uni.showToast({
									title: "已生成",
									icon: "success",
									duration: 2000
								})
								this.show = true;
							}
							else{
								uni.showToast({
									icon: "none",
									title: "请不要只输入敏感信息或表情！"
								})
							}
						}
					})
				}
			},
			publish() { //发表，成功后返回与我相关
				if (this.passageContent == "") {
					uni.showToast({
						icon: "error",
						title: '内容不能为空!'
					});
				} else if (this.current.src == "") {
					uni.showToast({
						title: '请先生成音频!'
					});
				} else {
					let passTitle = this.title;
					let passAb = this.abstract;
					let text = this.passageContent;
					let passTts = this.current.src;
					let _this = this;
					let Token = uni.getStorageSync('login_token');
					uni.request({ //请求发表文章
						url: 'http://106.14.62.110:8080/sound/post',
						method: "POST",
						data: {
							token: Token,
							postContent: text,
							postSummary: passAb,
							postTitle: passTitle,
							postTts: passTts
						},
						success: (res) => {
							if ("success" in res.data) {
								uni.showToast({
									icon: "success",
									title: "发表成功!",
									duration: 1000,
									success: function() {
										setTimeout(function() {
											uni.switchTab({
												url: './myselfMain',
											})
										}, 2000);
									}
								})
							} else {
								uni.showToast({
									icon: "error",
									title: "发表失败"
								})
							}
						}
					});
				}
			},
			play() { //播放

			},

			pause() { //暂停

			}
		}
	}
</script>

<style>
	.player {
		margin: 5% 2% 5% 2%;
		display: flex;
		flex-direction: row;
		justify-content: center;

	}

	.voiceChoice {
		margin-left: 5%;
		margin-right: 5%;
		margin-top: 5%;

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

	/*生成音频按钮*/
	.voiceGenerate {
		margin-left: 30%;
		margin-right: 30%;
		margin-top: 5%;
	}
</style>
