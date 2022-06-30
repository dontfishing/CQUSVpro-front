<template>
	<view>
		<!--语速滑动条-->
		<view class="sliderSet">
			<text class="btnText">语速</text>
			<slider :value="speed" min="0" max="15" @change="sliderChangeS" show-value="true" />
		</view>
		<!--音调滑动条-->
		<view class="sliderSet">
			<text class="btnText">音调</text>
			<slider :value="tune" min="0" max="15" @change="sliderChangeT" show-value="true" />
		</view>
		<!--音量滑动条-->
		<view class="sliderSet">
			<text class="btnText">音量</text>
			<slider :value="volume" min="0" max="9" @change="sliderChangeV" show-value="true" />
		</view></br>
		<!--音色选择单元格-->
		<u-cell-group>
			<u-cell title="音色选择" isLink @click="voiceChoice()" :value="timbreDisplay">
				<u-icon slot="icon" size="30" name="volume-fill"></u-icon>
			</u-cell>
		</u-cell-group>
		<!--生成试听音频按钮-->
		<view class="tryListen">
			<u-button text="生成试听音频" type="primary" @click="tryGenerate()"></u-button>
		</view>
		<!-- 试听 -->
		<view class="player">
			<audio style="text-align: left" :src="testLis.src" :poster="testLis.poster" :name="testLis.name"
				:author="testLis.author" controls @pause="pause()" @play="play()"></audio>
		</view>
		<!--保存按钮-->
		<view class="btn">
			<u-button text="保存" type="primary" @click="allSet()"></u-button>
		</view>
		<!--音色选择弹出层-->
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
				timbreDisplay: '度小美', //单元格右边显示选择的音色，默认音色1
				speed: 5, //语速
				tune: 5, //音调
				volume: 5, //音量 
				timbre: 0, //音色
				baseList: [{ //音色选择九宫格
						name: 'volume-fill',
						title: '度小美' //度小美
					},
					{
						name: 'volume-fill',
						title: '度小宇' //度小宇
					},
					{
						name: 'volume-fill',
						title: '度逍遥' //度逍遥
					},
					{
						name: 'volume-fill',
						title: '度丫丫' //度丫丫
					},
					{
						name: 'volume-fill',
						title: '度小娇' //度小娇
					},
					{
						name: 'volume-fill',
						title: '度米朵' //度米朵
					},
					{
						name: 'volume-fill',
						title: '度博文' //度博文
					},
					{
						name: 'volume-fill',
						title: '度小童' //度小童
					},
					{
						name: 'volume-fill',
						title: '度小萌' //度小萌
					}
				],
				testLis: { //播放器ui元素的来源
					src: "",
					poster: "https://bjetxgzv.cdn.bspapp.com/VKCEYUGU-uni-app-doc/7fbf26a0-4f4a-11eb-b680-7980c8a877b8.png",
					name: "试听",
					author: "试听文本"
				}
			}
		},
		
		onLoad() { //刚进入界面
			let _this = this;
			try { //获取缓存成功
				let voiceSet = uni.getStorageSync('voice_setting');
				if(voiceSet) {
					_this.speed = voiceSet.ttsSpd;
					_this.tune = voiceSet.ttsPit;
					_this.volume = voiceSet.ttsPit;
					_this.timbre = voiceSet.ttsPit;
					_this.generate();
				}
			} catch(e) { //获取缓存失败返回默认值
				_this.speed = 5
				_this.tune = 5
				_this.volume = 5
				_this.timbre = 0
				_this.generate();
			}
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
				let passPer = _this.timbre;
				let passSpd = _this.speed;
				let passPit = _this.tune;
				let passVol = _this.volume;
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
							icon: "none",
							duration: 2000
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
				this.timbre = name;
				this.timbreDisplay = this.baseList[name].title;
			},

			allSet() { //保存之后传递参数
				let _this = this;
				let loginSet = uni.getStorageSync('login_token');
				_this.token = loginSet;
				var voiceSetting = { //语音设置
					ttsSpd: _this.speed,
					ttsPit: _this.tune,
					ttsVol: _this.volume,
					ttsPer: _this.timbre
				};
				uni.setStorage({
					key: 'voice_setting',
					data: voiceSetting,
					success() {}
				});
				console.log(_this.speed);
				uni.request({
					url: 'http://106.14.62.110:8080/sound/setting',
					method: "POST",
					data: {
						token: _this.token,
						ttsSpd: _this.speed,
						ttsPit: _this.tune,
						ttsVol: _this.volume,
						ttsPer: _this.timbre
					},
					success: res => {
						if (res.statusCode == 404) {
							uni.showToast({
								icon: "error",
								title: "404 not found"
							})
						} else if ("error" in res.data) {
							uni.showToast({
								icon: "error",
								title: "设置失败"
							})
						} else {
							uni.showToast({
								icon: "success",
								title: "设置成功"
							})
						}
					}
				})

			},

			play() {}, //播放

			pause() {}, //暂停

			tryGenerate() { //点击生成试听音频按钮后触发，生成新的音频
				this.generate();
			}
		}
	}
</script>

<style>
	.btnText {
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
		margin-left: 30%;
		margin-right: 30%;
	}

	.tryListen {
		margin-top: 10%;
		margin-left: 30%;
		margin-right: 30%;
		margin-bottom: 10%;
	}
</style>
