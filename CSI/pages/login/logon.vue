<template>
	<view class="reg">
		<!-- style="{background: 'url('+imageURL+')'}" -->
		<!-- 如果是设置background-image则写成：<view class="content" :style="{backgroundImage: 'url('+imageURL+')'}"> -->
		<!-- 注册基本信息填入 -->
		<view class="info">

			<!-- 输入信息文字提示 -->
			<view class="infoVal">
				<text>账号</text></br>
				<text>密码</text></br>
				<text>确认密码</text></br>
				<text>性别</text></br>
				<text>生日</text></br>
				<text>邮箱</text>
			</view>

			<!-- 输入框 -->
			<view class="input">
				<!-- 用户名 -->
				<u--input class="infoAc" placeholder="4-16位,大小写字母、数字" border="surround" v-model="valueAc">
				</u--input></br>
				<!-- 密码 -->
				<u--input class="infoPsw" type="password" placeholder="6-16位,大小写字母、数字" border="surround"
					v-model="valuePsw"></u--input></br>

				<!-- 确认密码 -->
				<u--input class="infoPwAgain" type="password" placeholder="确认密码" border="surround"
					v-model="valuePwAgain">
				</u--input></br>
				<!-- 性别 -->
				<u-radio-group v-model="valueGend" placement="row">
					<u-radio :customStyle="{margin:'10px 50% 10px 0px'}" v-for="(item, index) in genderList"
						:key="index" :label="item.name" :name="item.name">
					</u-radio>
				</u-radio-group>
				<!-- <u--input class="infoGend" placeholder="性别" border="surround" v-model="valueGend"></br>
				</u--input> -->
				</br>
				<!-- 生日 -->
				<u--input class="infoAge" placeholder="生日" border="surround" v-model="valueAge" @focus="open()">
				</u--input></br>
				<!-- 邮箱 -->
				<u--input class="infoEmail" placeholder="邮箱" border="surround" v-model="valueEmail">
				</u--input>

			</view>
		</view>
		<!--注册按钮-->
		<view class="btnRgs">
			<u-button type="primary" color="#8967D3" text="注册" @click="register()"></u-button>
		</view>
		<uni-calendar ref="calendar" class="uni-calendar--hook" :clear-date="true" :date="info.date"
			:insert="info.insert" :lunar="info.lunar" :startDate="info.startDate" :endDate="info.endDate"
			:range="info.range" @confirm="confirm" @close="close" />
	</view>
</template>

<script>
	/**
	 * 获取任意时间
	 */
	function getDate(date, AddDayCount = 0) {
		if (!date) {
			date = new Date()
		}
		if (typeof date !== 'object') {
			date = date.replace(/-/g, '/')
		}
		const dd = new Date(date)

		dd.setDate(dd.getDate() + AddDayCount) // 获取AddDayCount天后的日期

		const y = dd.getFullYear()
		const m = dd.getMonth() + 1 < 10 ? '0' + (dd.getMonth() + 1) : dd.getMonth() + 1 // 获取当前月份的日期，不足10补0
		const d = dd.getDate() < 10 ? '0' + dd.getDate() : dd.getDate() // 获取当前几号，不足10补0
		return {
			fullDate: y + '-' + m + '-' + d,
			year: y,
			month: m,
			date: d,
			day: dd.getDay()
		}
	}
	export default {
		components: {},
		data() {
			return {
				info: { //日历的信息
					lunar: false, //不显示农历
					range: false, //只能选择一个日期
					insert: false,
					selected: []
				},
				valueAc: "", //账号
				valuePsw: "", //密码
				valuePwAgain: "", //确认密码
				valueGend: "", //性别
				valueAge: "",
				valueEmail: "", //邮箱
				genderList: [{
					name: '男',
					disabled: false
				}, {
					name: '女',
					disabled: true
				}],
				valueGend: '男',
				// imageURL: 'E:\\1\\codelearn\\前端\\0-init\\web\\data\\pic\\pic-1.png'
			};
		},
		methods: {

			open() {
				this.$refs.calendar.open()
			},

			confirm(e) {
				this.valueAge = e["fulldate"];
			},
			
			close() {
				
			},

			register() {
				var data = {
					useraccount: this.valueAc,
					password: this.valuePsw,
					passwordAgain: this.valuePwAgain,
					gender: this.valueGend,
					birthday: this.valueAge,
					email: this.valueEmail
				};
				// 切换性别
				if (this.valueGend == "女") {
					this.valueGend = "f";
				} else {
					this.valueGend = "m";
				}

				// 判断输入不为空
				if (data.useraccount == "" || data.password == "" || data.passwordAgain == "" || data.valueAge == "" ||
					data
					.valueEmail == "") {
					uni.showToast({
						icon: 'error',
						title: '输入不可以为空'
					});
				}

				// 判断用户名是否符号要求
				else if (!(/^[A-Za-z\d]{5,24}$/.test(this.valueAc)) || this.valueAc.length < 4 || this.valueAc.length >
					16) {
					uni.showToast({
						icon: 'error',
						title: '用户名不符合要求'
					});
				}


				// 判断密码是否符号要求
				else if (!(/^[A-Za-z\d]{6,16}$/.test(this.valuePsw))) {
					uni.showToast({
						icon: 'error',
						title: '密码不符合要求'
					});
				}

				// 判断邮箱是否符号要求
				else if (!(/^[a-zA-Z0-9_-]+@[a-zA-Z0-9_-]+(\.[a-zA-Z0-9_-]+)+$/.test(this.valueEmail))) {
					uni.showToast({
						icon: 'error',
						title: '邮箱不符合要求'
					});
				}

				// 判断确认密码与密码是否一致
				else if (data.password != data.passwordAgain) {
					uni.showToast({
						icon: 'error',
						title: '确认密码与密码不一致'
					});
				} else {
					uni.request({
						url: 'http://106.14.62.110:8080/userRegister', //api地址
						method: "POST",
						data: {
							useraccount: this.valueAc,
							password: this.valuePsw,
							gender: this.valueGend,
							birthday: this.valueAge,
							email: this.valueEmail
						},
						success: res => {
							if (res.statusCode == 404) { //返回的状态码
								uni.showToast({
									icon: 'error',
									title: '网页失踪了',
								});
							}
							// 用户名已存在
							else if (res.data == 2) {
								uni.showToast({
									icon: 'error',
									title: '用户名已被占用'
								});
							}
							// 服务器错误
							else if (res.data == 1) {
								uni.showToast({
									icon: 'error',
									title: '发生错误，请稍后再试'
								});
							} else {

								uni.showToast({
									icon: 'none',
									title: '注册成功',
									duration: 2000
								});

								uni.navigateTo({
									url: '/pages/login/login'
								});
							}
						},

						fail: () => {

						},

						complete: () => {

						}
					});
				}
			}
			// goto(url) {}
		}
	}
</script>

<style>
	/* 总页面 */

	.reg {
		color: black;
		text-decoration-color: white;
	}

	/* 注册标题 */
	.title {
		margin: 10%;
		margin-left: 36%;
		font-size: 50px;
	}


	/* 注册信息总界面 */
	.info {
		display: flex;
		flex-direction: row;
		align-items: center;
		justify-content: center;
		margin-top: 25%;
	}

	/* 注册信息文字提示 */
	.infoVal {
		display: flex;
		flex-direction: column;

		font-size: 45rpx;
		padding: 10px 10px 10px 10px;
		align-items: center;
		justify-content: center;
	}

	/* 输入框 */
	.input {
		display: flex;
		flex-direction: column;
		padding: 10px 10px 10px 10px;

		size: 20rpx;

	}

	/* 注册按钮 */
	.btnRgs {
		margin-top: 40px;
		margin-right: 10%;
		margin-bottom: 10px;
		margin-left: 10%;
		text-align: right;

	}
</style>
