<template>
	<view>
		<view class="sliderSet">
			<text>语速</text>
			<slider value="5" min="0" max="15" @change="sliderChangeS" show-value="true" />
		</view>
		<view class="sliderSet">
			<text>音调</text>
			<slider value="5" min="0" max="15" @change="sliderChangeT" show-value="true" />
		</view>
		<view class="sliderSet">
			<text>音量</text>
			<slider value="5" min="0" max="9" @change="sliderChangeV" show-value="true" />
		</view></br>
		<u-cell-group>
			<u-cell title="音色选择" isLink @click="voiceChoice()" :value="timbreDisplay">
				<u-icon slot="icon" size="30" name="volume-fill"></u-icon>
				<!-- <u-badge count="99" :absolute="false" slot="right-icon"></u-badge> -->
			</u-cell>
		</u-cell-group>
		<u-button text="确定" type="primary" @click="allSet()"></u-button>
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
				volume: 4, //音量 
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
				]
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
					data : voiceSetting,
					success() {
						console.log("success!");
						uni.showToast({
							title: "保存成功",
							icon: "success"
						})
					}
				})
			}
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

	.u-button {
		margin-top: 30%;
		margin-left: 35%;
		width: 30%;
	}
</style>
