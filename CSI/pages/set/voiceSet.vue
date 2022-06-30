<template>
	<view>
		<view class="sliderSet">
			<text>语速</text>
			<slider :value="speed" min="0" max="15" @change="sliderChangeS" show-value="true" />
		</view>
		<view class="sliderSet">
			<text>音调</text>
			<slider :value="tune" min="0" max="15" @change="sliderChangeT" show-value="true" />
		</view>
		<view class="sliderSet">
			<text>音量</text>
			<slider :value="volume" min="0" max="9" @change="sliderChangeV" show-value="true" />
		</view></br>
		<u-cell-group>
			<u-cell title="音色选择" isLink @click="voiceChoice()" :value="timbreDisplay">
				<u-icon slot="icon" size="30" name="volume-fill"></u-icon>
			</u-cell>
		</u-cell-group>
		<!-- 试听 -->
		<view class="player">
			<audio style="text-align: left" :src="testLis.src" :poster="testLis.poster" :name="testLis.name"
				:author="testLis.author" controls @pause="pause()" @play="play()"></audio>
		</view>
		<view class="btn">
			<u-button text="确定" type="primary" @click="allSet()"></u-button>
		</view>
		<u-popup :show="show" @close="close" @open="open">
			<view>
				<u-grid :border="true" @click="timbreChoice" v-model="timbreDisplay">
					<u-grid-item v-for="(baseListItem,baseListIndex) in baseList" :key="baseListIndex">
						<u-icon :customStyle="{paddingTop:20+'rpx'}" :name="baseListItem.name" :size="22"></u-icon>
						<text class="grid-text">{{baseListItem.title}}</text>
					</u-grid-item>
					<u-toast ref="uToast" />
				</u-grid>
			</view>
		</u-popup>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				show: false, //弹出层默认不显示
				timbreDisplay: '音色1', //单元格右边显示选择的音色，默认音色1
				speed: 5, //语速
				tune: 5, //音调
				volume: 5, //音量 
				timbre: 0, //音色
				baseList: [{
						name: 'volume-fill',
						title: '音色1' //度小美
					},
					{
						name: 'volume-fill',
						title: '音色2' //度小宇
					},
					{
						name: 'volume-fill',
						title: '音色3' //度逍遥
					},
					{
						name: 'volume-fill',
						title: '音色4' //度丫丫
					},
					{
						name: 'volume-fill',
						title: '音色5' //度小娇
					},
					{
						name: 'volume-fill',
						title: '音色6' //度米朵
					},
					{
						name: 'volume-fill',
						title: '音色7' //度博文
					},
					{
						name: 'volume-fill',
						title: '音色8' //度小童
					},
					{
						name: 'volume-fill',
						title: '音色9' //度小萌
					}
				],
				testLis: {
					src: "",
					poster: "https://bjetxgzv.cdn.bspapp.com/VKCEYUGU-uni-app-doc/7fbf26a0-4f4a-11eb-b680-7980c8a877b8.png",
					name: "试听",
					author: "试听文本"
				}
			}
		},
		onLoad() {
			let _this = this;
			uni.getStorage({
				key: "voice_setting",
				success(res) {
					console.log(JSON.stringify(res.data));
					_this.speed = res.data.ttsSpd;
					_this.tune = res.data.ttsPit;
					_this.volume = res.data.ttsVol;
					_this.timbre = res.data.ttsPer;
					_this.generate();
				},
				fail() {
					_this.speed = 5;
					_this.tune = 5;
					_this.volume = 5;
					_this.timbre = 4;
					_this.generate();
				}
			})
		},
		methods: {
			sliderChangeS(e) { //传语速设置
				this.speed = e.detail.value;
			},

			sliderChangeT(e) { //传音调设置
				this.tune = e.detail.value;
			},

			sliderChangeV(e) { //传音量设置
				this.volume = e.detail.value;
			},

			voiceChoice() { //点击单元格弹出音色选择
				this.show = true;
			},
			generate() { //生成试听音频
				let _this = this;
				let passPer = this.timbre;
				let passSpd = this.speed;
				let passPit = this.tune;
				let passVol = this.volume;
				let passContent = "衬衫的价格为九镑十五便士，所以你选择C项，并将其标在试卷上。";
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
						_this.testLis.src = res.data.postTts;
						uni.showToast({
							title: "修改成功，已生成试听音频",
							icon: "success",
							duration: 4000
						});
					}
				})

			},
			open() {

			},

			close() {
				this.show = false;
			},

			timbreChoice(name) { //传音色的选择
				this.$refs.uToast.success(`设置成功`);
				this.show = false;
				console.log(name);
				this.timbre = name;
				this.timbreDisplay = "音色" + (name + 1);
			},

			allSet() {
				var voiceSetting = { //语音设置
					ttsSpd: this.speed,
					ttsPit: this.tune,
					ttsVol: this.volume,
					ttsPer: this.timbre
				};
				uni.setStorage({
					key: 'voice_setting',
					data: voiceSetting,
					success() {}
				});
				this.generate(); //生成试听
			},

			play() {},

			pause() {},
		}
	}
</script>

<style>
	text {
		margin-top: 6%;
		margin-right: 2%;
		margin-left: 2%;
		font-size: 20px;
	}

	.sliderSet {
		border-radius: 5%;
		margin-left: 3%;
		margin-right: 3%;
		margin-top: 5%;
	}

	.grid-text {
		font-size: 14px;
		color: #909399;
		padding: 10rpx 0 20rpx 0rpx;
		/* #ifndef APP-PLUS */
		box-sizing: border-box;
		/* #endif */
	}

	.player {
		margin-top: 15px;
		margin-left: 20%;
	}

	.btn {
		margin-top: 10%;
	}
</style>
