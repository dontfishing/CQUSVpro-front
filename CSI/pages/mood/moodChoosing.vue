<template>
	<view class="labelChoosing">
		<!-- 标签选择区域 -->
		<view class="labelChoice">

			<!-- 年龄选项 -->
			<view class="btnAge">
				<view v-for="(item,index) in ageList" :key="index">
					<view class="btnAge">
						<u-button type="primary" :text="ageList[index]" :color="ColorList.age[index]"
							@click="ageChoosing(index)"></u-button>
					</view>
				</view>
			</view>

			<!-- 心情选项 -->
			<view class="btnMood">
				<view class="btnMoodR">
					<u-button type="primary" customStyle="margin:2% 2% 2% 2%" :text="moodList[0]"
						:color="ColorList.mood[0]" @click="moodChoosing(0)">
					</u-button>
					<u-button type="primary" customStyle="margin:2% 2% 2% 2%" :text="moodList[1]"
						:color="ColorList.mood[1]" @click="moodChoosing(1)">
					</u-button>
					<u-button type="primary" customStyle="margin:2% 2% 2% 2%" :text="moodList[2]"
						:color="ColorList.mood[2]" @click="moodChoosing(2)">
					</u-button>
					<u-button type="primary" customStyle="margin:2% 2% 2% 2%" :text="moodList[3]"
						:color="ColorList.mood[3]" @click="moodChoosing(3)">
					</u-button>
				</view>
				<view class="btnMoodR">
					<u-button type="primary" customStyle="margin:2% 2% 2% 2%" :text="moodList[4]"
						:color="ColorList.mood[4]" @click="moodChoosing(4)">
					</u-button>
					<u-button type="primary" customStyle="margin:2% 2% 2% 2%" :text="moodList[5]"
						:color="ColorList.mood[5]" @click="moodChoosing(5)">
					</u-button>
					<u-button type="primary" customStyle="margin:2% 2% 2% 2%" :text="moodList[6]"
						:color="ColorList.mood[6]" @click="moodChoosing(6)">
					</u-button>
				</view>

				<!-- <view v-for="(item,index) in moodList" :key="index"> -->
				<!-- <view class="btnMood"> -->
				<!-- <u-button type="primary" :text= "moodList[index]" :color="ColorList.mood[index]" -->
				<!-- @click="moodChoosing(index)"></u-button> -->
				<!-- </view> -->
				<!-- </view> -->
			</view>

			<!-- 脸型选项 -->
			<view class="btnShape">
				<view v-for="(item,index) in shapeList" :key="index">
					<view class="btnShape">
						<u-button type="primary" :text="shapeList[index]" :color="ColorList.shape[index]"
							@click="shapeChoosing(index)"></u-button>
					</view>
				</view>
			</view>
		</view>

		<!-- 文字提示区域 -->
		<view class="selectedSumText">
			<text>已选择数：{{labelChoseSum}}/5</text>
		</view>

		<!-- 按钮区域 -->
		<view class="Button">
			<view class="btnConfirm">
				<u-button type="primary" text="确认" color="#1f1f1f" :plain="true" @click="confirm()"></u-button>
			</view>

			<view class="btnReset">
				<u-button type="primary" text="重置" color="#1f1f1f" :plain="true" @click="reset()"></u-button>
			</view>

		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				labelChoseSum: 0, //已选择标签数量
				ageList: ["0-12", "13-28", "29-50", "50+"], //年龄标签选择列表
				moodList: ["开心", "伤心", "愤怒", "自然", "恐惧", "厌恶", "惊喜"], //心情标签选择列表
				shapeList: ["方形脸", "马脸", "瓜子脸", "心形脸", "圆脸"], //脸型标签选择列表
				ColorList: {
					age: ['#c4c6c9', '#c4c6c9', '#c4c6c9', '#c4c6c9'],
					mood: ['#c4c6c9', '#c4c6c9', '#c4c6c9', '#c4c6c9', '#c4c6c9', '#c4c6c9', '#c4c6c9'],
					shape: ['#c4c6c9', '#c4c6c9', '#c4c6c9', '#c4c6c9', '#c4c6c9']
				},
				selectedList: {
					age: [],
					mood: [],
					shape: []
				}
			}
		},

		onLoad() {
			let that = this;
			that.reset();
		},

		methods: {
			ageChoosing(index) {
				if (this.ColorList.age[index] == '#DFE1FF') {
					this.ColorList.age[index] = '#c4c6c9';
					this.labelChoseSum--;
					this.selectedList.age
				} else {
					if (this.labelChoseSum == 5) {
						uni.showToast({
							icon: 'error',
							title: '标签已选满'
						});
					} else {
						this.ColorList.age[index] = '#DFE1FF';
						this.labelChoseSum++;
					}

				}
			},
			moodChoosing(index) {
				if (this.ColorList.mood[index] == '#AACFCF') {
					this.ColorList.mood[index] = '#c4c6c9';
					this.labelChoseSum--;
				} else {
					if (this.labelChoseSum == 5) {
						uni.showToast({
							icon: 'error',
							title: '标签已选满'
						});
					} else {
						this.ColorList.mood[index] = '#AACFCF';
						this.labelChoseSum++;
					}
				}
			},
			shapeChoosing(index) {
				if (this.ColorList.shape[index] == '#CD96CD') {
					this.ColorList.shape[index] = '#c4c6c9';
					this.labelChoseSum--;
				} else {
					if (this.labelChoseSum == 5) {
						uni.showToast({
							icon: 'error',
							title: '标签已选满'
						});
					} else {
						this.ColorList.shape[index] = '#CD96CD';
						this.labelChoseSum++;
					}
				}
			},

			confirm() { //确认
				this.selectedList = {
					age: [],
					mood: [],
					shape: []
				};
				var flag=1;
				for (var i = 0; i < this.ageList.length; i++) {
					if (this.ColorList.age[i] != '#c4c6c9') {
						this.selectedList.age.push(this.ageList[i]);
						flag=0;
					}
				}
				for (var i = 0; i < this.moodList.length; i++) {
					if (this.ColorList.mood[i] != '#c4c6c9') {
						this.selectedList.mood.push(this.moodList[i]);
						flag=0;
					}
				}
				for (var i = 0; i < this.shapeList.length; i++) {
					if (this.ColorList.shape[i] != '#c4c6c9') {
						this.selectedList.shape.push(this.shapeList[i]);
						flag=0;
					}
				}

				if (flag==1) {
					this.selectedList.mood.push("life");
				}

				let list = this.selectedList;
				uni.setStorage({
					key: 'selectedList',
					data: list,
					success: function() {
						uni.navigateTo({
							url: 'moodMain'
						});
					}
				});

			},
			reset() { //重置

				this.labelChoseSum = 0;
				this.ColorList = {
					age: ['#c4c6c9', '#c4c6c9', '#c4c6c9', '#c4c6c9'],
					mood: ['#c4c6c9', '#c4c6c9', '#c4c6c9', '#c4c6c9', '#c4c6c9', '#c4c6c9', '#c4c6c9'],
					shape: ['#c4c6c9', '#c4c6c9', '#c4c6c9', '#c4c6c9', '#c4c6c9']
				};
				this.selectedList = {
					age: [],
					mood: [],
					shape: []
				}

			}
		}
	}
</script>

<style>
	/* 整个页面 */
	.labelChoosing {
		display: flex;
		flex-direction: column;

		/* align-items: center; */
		/* justify-content: center; */

	}

	.labelChoice {
		margin: 5% 5% 5% 5%;

	}

	.btnAge {
		display: flex;
		flex-direction: row;
		align-items: center;
		justify-content: center;
		margin: 2% 10% 5% 10%;
	}

	.btnMood {
		display: flex;
		flex-direction: column;
		margin: 5% 5% 5% 5%;

	}

	.btnMoodR {
		display: flex;
		flex-direction: row;
		margin: 1% 5% 1% 5%;

	}

	.btnShape {
		display: flex;
		flex-direction: row;
		margin: 5% 5% 5% 5%;
		align-items: center;
		justify-content: center;

	}

	.selectedSumText {
		display: flex;
		flex-direction: row;
		align-items: center;
		justify-content: center;

	}

	.Button {
		margin: 5% 0% 5% 0%;
		/* align-items: center; */
		/* justify-content: center; */

	}

	.btnConfirm {
		margin: 5% 20% 10% 20%;
		width: 60%;
	}

	.btnReset {
		margin: 10% 20% 5% 20%;
		width: 60%;

	}

	/* 	
	<u-button type="primary" text="0-12" :color="ColorList.age[0]" @click="ageChoosing(0)"></u-button>
		<u-button type="primary" text="13-28" :color="ColorList.age[1]" @click="ageChoosing(1)"></u-button>
		<u-button type="primary" text="29-50" :color="ColorList.age[2]" @click="ageChoosing(2)"></u-button>
		<u-button type="primary" text="50+" :color="ColorList.age[3]" @click="ageChoosing(3)"></u-button>
	
	
	<u-button type="primary" text="0-12" :color="ColorList.mood[0]" @click="moodChoosing(0)"></u-button>
	<u-button type="primary" text="13-28" :color="ColorList.mood[1]" @click="moodChoosing(1)"></u-button>
	<u-button type="primary" text="29-50" :color="ColorList.mood[2]" @click="moodChoosing(2)"></u-button>
	<u-button type="primary" text="50+" :color="ColorList.mood[3]" @click="moodChoosing(3)"></u-button>
	
	<u-button type="primary" text="0-12" :color="ColorList.shape[0]" @click="shapeChoosing(0)"></u-button>
	<u-button type="primary" text="13-28" :color="ColorList.shape[1]" @click="shapeChoosing(1)">
	</u-button>
	<u-button type="primary" text="29-50" :color="ColorList.shape[2]" @click="shapeChoosing(2)">
	</u-button>
	<u-button type="primary" text="50+" :color="ColorList.shape[3]" @click="shapeChoosing(3)"></u-button>
	
 */
</style>
