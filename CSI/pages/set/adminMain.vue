<template>
	<view>

		<view class="sexMain">
			<view class="title">性别分布</view>
			<qiun-data-charts type="ring" :opts="sexOpts" :chartData="sexData"></qiun-data-charts>
		</view>
		<view class="ageMain">
			<view class="title">年龄分布</view>
			<qiun-data-charts type="mount" :opts="ageOpts" :chartData="ageData" />
		</view>
		<view class="moodMain">
			<view class="title">用户发表心情分布</view>
			<qiun-data-charts type="bar" :opts="moodOpts" :chartData="moodData" />
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				sexData: {	//性别数据
					series: [{
						data: [{
							"name": "男性",
							"value": 0
						}, {
							"name": "女性",
							"value": 0
						}]
					}]
				},
				sexOpts: {	//性别图片
					rotate: false,
					rotateLock: false,
					color: ["#1890FF", "#EE6666"],
					padding: [5, 5, 5, 5],
					dataLabel: true,
					legend: {
						show: true,
						position: "right",
						lineHeight: 25
					},
					title: {
						name: "用户",
						fontSize: 15,
						color: "#8967D3"
					},
					subtitle: {
						name: "男女比",
						fontSize: 15,
						color: "#8967D3"
					},
					extra: {
						ring: {
							ringWidth: 40,
							activeOpacity: 0.5,
							activeRadius: 10,
							offsetAngle: 0,
							labelWidth: 15,
							border: false,
							borderWidth: 3,
							borderColor: "#FFFFFF"
						}
					}
				},
				ageData: {	//年龄数据
					series: [{
						data: [{
							"name": "0-12岁",
							"value": 0
						}, {
							"name": "13-28岁",
							"value": 0
						}, {
							"name": "28-50岁",
							"value": 0
						}, {
							"name": "50岁以上",
							"value": 0
						}]
					}]

				},
				ageOpts: {	//年龄图片
					color: ["#1890FF", "#EE6666", "#91CB74", "#FAC858"],
					padding: [15, 15, 0, 5],
					legend: {},
					xAxis: {
						disableGrid: true
					},
					yAxis: {
						data: [{
							min: 0
						}]
					},
					extra: {
						mount: {
							type: "bar",
							widthRatio: 0.8
						}
					}
				},
				moodData: {	//心情数据
					categories: ["开心", "悲伤", "厌恶", "恐惧", "惊喜", "自然", "愤怒", "life"],
					series: [{
						name: "人数",
						data: [0, 0, 0, 0, 0, 0, 0, 0]
					}]
				},
				moodOpts: {	//心情图片
					color: ["#73C0DE"],
					padding: [15, 30, 0, 5],
					legend: {},
					xAxis: {
						boundaryGap: "justify",
						disableGrid: false,
						min: 0,
						axisLine: false,
						max: 70
					},
					yAxis: {},
					extra: {
						bar: {
							type: "stack",
							width: 30,
							meterBorde: 1,
							meterFillColor: "#FFFFFF",
							activeBgColor: "#000000",
							activeBgOpacity: 0.08,
							categoryGap: 2
						}
					}
				}
			}
		},
		onLoad() {
			let _this=this;
			let Token = uni.getStorageSync('login_token');
			uni.request({
				url: 'http://106.14.62.110:8080/admin/usersInfo',
				data: {
					token: Token
				},
				method: 'POST',
				success: (res) => {
					console.log(JSON.stringify(res.data));
					_this.getSex(res.data);
					_this.getAge(res.data);
					_this.getMood(res.data);
				}
			})
		},
		methods: {
			getSex(ob) {	//获取性别
				this.sexData.series[0].data[0].value = ob.male;
				this.sexData.series[0].data[1].value = ob.female;
			},
			getAge(ob) {	//获取年龄
				this.ageData.series[0].data[0].value = ob.child;
				this.ageData.series[0].data[1].value = ob.teenager;
				this.ageData.series[0].data[2].value = ob.middleAge;
				this.ageData.series[0].data[3].value = ob.elder;
			},
			getMood(ob) {	//获取心情
				let tmp = [];
				tmp.push(ob.happy);
				tmp.push(ob.sad);
				tmp.push(ob.hate);
				tmp.push(ob.fear);
				tmp.push(ob.surprise);
				tmp.push(ob.natural);
				tmp.push(ob.angry);
				tmp.push(ob.life);
				this.moodData.series[0].data = tmp;
			}
		},
	}
</script>

<style>
	.sexMain {
		width: 100%;
		height: 290px;
		margin-top: 10px;
	}

	.ageMain {
		width: 100%;
		height: 350px;
		margin-top: 10px;
	}

	.moodMain {
		width: 100%;
		height: 380px;
		margin-top: 50px;
	}

	.title {
		font-weight: bold;
		font-size: 30px;
		text-align: center;
	}
</style>
