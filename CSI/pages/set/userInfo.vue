<template>
	<view>
		<u-cell-group>
			<!--界面单元格-->
			<u-cell title="用户名" isLink @click="pop(0)" :value="Info.userName" icon="/static/user.png" size="large">
			</u-cell>
			<u-cell title="性别" isLink @click="showSexPop(1)" :value="Info.gender" icon="/static/gender.png" size="large">
			</u-cell>
			<u-cell title="生日" isLink @click="open" :value="Info.birth" icon="/static/birthday.png" size="large">
			</u-cell>
			<u-cell title="星座" isLink :value="Info.star" icon="/static/star.png" size="large">
			</u-cell>
			<u-cell title="年龄" isLink :value="Info.age" icon="/static/age.png" size="large">
			</u-cell>
			<u-cell title="所在地" isLink @click="pop(5)" :value="Info.addr" icon="/static/home.png" size="large">
			</u-cell>
			<u-cell title="职业" isLink @click="pop(6)" :value="Info.job" icon="/static/job.png" size="large">
			</u-cell>
			<u-cell title="邮箱" isLink @click="pop(7)" :value="Info.email" icon="/static/mailbox.png" size="large">
			</u-cell>
			<u-cell title="密码" isLink @click="showPasswordPop()" icon="/static/password.png" size="large">
			</u-cell>
		</u-cell-group>
		<view class="popLayer">
			<!--除密码,性别,生日外其余单元格的弹出层-->
			<!--用showCommon控制是否弹出-->
			<u-popup :show="showCommom" @close="close" @open="openCommom" mode="center">
				<view class="popInput">
					<!--指示和输入-->
					<text>{{modifyValue[popNum].newInfo}}</text>
					<u-input v-model="modifyValue[popNum].newInput"></u-input>
				</view>
				<!--取消和确认按钮-->
				<view class="yesOrNot">
					<u-button class="cancleClass" type="primary" text="取消" shape="circle" @click="cancle()"
						size="small" color="#c4c6c9">
					</u-button>
					<u-button class="yesClass" type="primary" text="确定" shape="circle" @click="yes(modifyValue[popNum])"
						size="small">
					</u-button>
				</view>
			</u-popup>
		</view>
		<view>
			<!--性别单元格的弹出层-->
			<!--用showSex控制是否弹出-->
			<u-popup :show="showSex" @close="close" @open="openGender" mode="center">
				<view class="popInput">
					<!--指示和输入-->
					<text>{{modifyValue[popNum].newInfo}}</text>
					<u-radio-group style="width:240px" v-model="modifyValue[popNum].newInput" placement="row">
						<u-radio :customStyle="{margin:'10px 60% 10px 0px'}" v-for="(item, index) in genderList"
							:key="index" :label="item.name" :name="item.name">
						</u-radio>
					</u-radio-group>
				</view>
				<!--取消和确认按钮-->
				<view class="yesOrNot">
					<u-button class="cancleClass" type="primary" text="取消" shape="circle" @click="cancle()"
						size="small" color="#c4c6c9">
					</u-button>
					<u-button class="yesClass" type="primary" text="确定" shape="circle" @click="yes(modifyValue[popNum])"
						size="small">
					</u-button>
				</view>
			</u-popup>
		</view>
		<view class="popLayer">
			<!--=密码单元格的弹出层-->
			<!--用showPassword控制是否弹出-->
			<u-popup :show="showPassword" @close="close" @open="openPass" mode="center">
				<view class="popPassword">
					<view class="inputPassword">
						<text>旧密码</text>
						<u-input v-model="oldPassword"></u-input>
					</view>
					<view class="inputPassword">
						<text>新密码</text>
						<u-input v-model="password"></u-input>
					</view>
					<view class="inputPassword">
						<text>确认密码</text>
						<u-input v-model="passwordAgain"></u-input>
					</view>
					<!--取消和确认按钮-->
					<view class="yesOrNot">
						<u-button class="cancleClass" type="primary" text="取消" shape="circle" @click="cancle()"
							size="small" color="#c4c6c9">
						</u-button>
						<u-button class="yesClass" type="primary" text="确定" shape="circle" @click="yesPass()"
							size="small">
						</u-button>
					</view>
				</view>
			</u-popup>
		</view>
		<!--选择生日-->
		<view>
			<uni-calendar ref="calendar" class="uni-calendar--hook" :clear-date="true" :date="info.date"
				:insert="info.insert" :lunar="info.lunar" :startDate="info.startDate" :endDate="info.endDate"
				:range="info.range" @confirm="confirm" @close="close" />
		</view>
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
				info: { //日历弹出层的信息
					lunar: false, //不显示农历
					range: false, //使能选择一天
					insert: false,
					selected: []
				},
				Info: { //当前用户信息
					userName: "", //用户名
					gender: "", //性别
					birth: "", //生日
					star: "", //星座
					age: "", //年龄
					addr: "", //地址
					job: "", //职业
					email: "", //邮箱
				},
				token: "",
				showCommom: false, //除密码和性别外的弹出层控制
				showSex: false, //性别的弹出层控制
				showPassword: false, //密码的弹出层控制
				popNum: 0, //索引
				oldPassword: "", //旧密码
				password: "", //输入的新密码
				passwordAgain: "", //再次输入的新密码
				genderList: [{
					name: '男',
					disabled: true
				}, {
					name: '女',
					disabled: false
				}], //性别列表
				modifyValue: [{ //输入绑定的对象
					newInfo: "新用户名",
					newInput: ""
				}, {
					newInfo: "性别",
					newInput: ""
				}, {
					newInfo: "生日",
					newInput: ""
				}, {
					newInfo: "星座",
					newInput: ""
				}, {
					newInfo: "年龄",
					newInput: ""
				}, {
					newInfo: "所在地",
					newInput: ""
				}, {
					newInfo: "职业",
					newInput: ""
				}, {
					newInfo: "邮箱",
					newInput: ""
				}]
			}
		},
		methods: {
			getInfo() { //获取当前信息
				var Token;
				uni.getStorage({
					key: 'login_token',
					success: function(res) {
						Token = res.data;
					}
				});
				console.log(Token);
				uni.request({
					url: 'http://106.14.62.110:8080/user/setting',
					method: "POST",
					data: {
						token: Token,
					},
					success: res => {
						if (res.statusCode == 404) {
							uni.showToast({
								icon: "error",
								title: "404 not found"
							})
						} else {
							this.Info.userName = res.data.userName;
							//this.Info.gender = res.data.userGender;
							this.Info.birth = res.data.userBirth;
							this.Info.star = res.data.userStar;
							this.Info.age = res.data.userAge + "";
							this.Info.addr = res.data.userAddr;
							this.Info.job = res.data.userJob;
							this.Info.email = res.data.userEmail;
							if(res.data.userGender == "f")
								this.Info.gender = "女";
							else
								this.Info.gender = "男";
						}
					}
				})
			},

			pop(num) { //弹出层信息根据传入的参数改变
				this.showCommom = true;
				this.showPassword = false;
				this.showSex = false;
				this.popNum = num;
			},

			showSexPop(num) { //弹出层信息根据传入的参数改变
				this.showCommom = false;
				this.showPassword = false;
				this.showSex = true;
				this.popNum = num;
				uni.getStorage({
					key: 'login_token',
					success: function(res) {
						this.token = res.data;
						console.log(this.token);
					}
				});
			},

			close() { //直接关闭弹出层
				this.showCommom = false;
				this.showPassword = false;
				this.showSex = false;
			},

			open() { //打开日历
				this.$refs.calendar.open()
			},

			openCommom() {

			},

			openGender() {

			},

			openPass() {

			},

			judgeVoid(str) {
				if (str == "") {
					uni.showToast({
						icon: "none",
						title: "输入不能为空"
					})
					return 1;
				} else {
					return 0;
				}
			},

			cancle() { //直接关闭弹出层
				this.showCommom = false;
				this.showPassword = false;
				this.showSex = false;
			},

			onLoad() { //每次加载都会重新刷新
				let that = this;
				that.getInfo();
			},

			yes(modifyObj) { //点击确认，传递参数
				this.showCommom = false;
				this.showPassword = false;
				var Token;
				uni.getStorage({ //获取login的token
					key: 'login_token',
					success: function(res) {
						Token = res.data;
					}
				});
				console.log(Token);
				if (this.judgeVoid(modifyObj.newInput)){
					this.showCommom = true;
					return;}
				else {
					switch (this.popNum) { //根据popNum判断修改的哪个信息
						case 0: //用户名
							uni.request({
								url: 'http://106.14.62.110:8080/user/changeInfo/name',
								method: "POST",
								data: {
									token: Token,
									name: modifyObj.newInput
								},
								success: res => {
									console.log(JSON.stringify(res.data));
									if (res.statusCode == 404) {
										uni.showToast({
											icon: "none",
											title: "修改失败"
										})
									} else if ("error" in res.data) {
										uni.showToast({
											icon: "none",
											title: "修改失败"
										})
									} else {
										uni.showToast({
											icon: "none",
											title: "修改成功"
										})
										this.Info.userName = res.data.name;
									}
								}
							});
							break;
						case 1: //性别
							var sex = "m";
							if (modifyObj.newInput == "女")
								sex = "f";
							uni.request({
								url: 'http://106.14.62.110:8080/user/changeInfo/gender',
								method: "POST",
								data: {
									token: Token,
									gender: sex
								},
								success: res => {
									console.log(JSON.stringify(res.data));
									if (res.statusCode == 404) {
										uni.showToast({
											icon: "none",
											title: "修改失败"
										})
									} else if ("error" in res.data) {
										uni.showToast({
											icon: "none",
											title: "修改失败"
										})
									} else {
										uni.showToast({
											icon: "none",
											title: "修改成功"
										})
										this.Info.gender = modifyObj.newInput;
										this.showSex = false;
									}

								}
							});
							break;
						case 5: //所在地
							uni.request({
								url: 'http://106.14.62.110:8080/user/changeInfo/address',
								method: "POST",
								data: {
									token: Token,
									address: modifyObj.newInput
								},
								success: res => {
									console.log(JSON.stringify(res.data));
									if (res.statusCode == 404) {
										uni.showToast({
											icon: "none",
											title: "修改失败"
										})
									} else if ("error" in res.data) {
										uni.showToast({
											icon: "none",
											title: "修改失败"
										})
									} else {
										uni.showToast({
											icon: "none",
											title: "修改成功",
										})
										this.Info.addr = res.data.address;
									}
								}
							});
							break;
						case 6: //职业
							uni.request({
								url: 'http://106.14.62.110:8080/user/changeInfo/job',
								method: "POST",
								data: {
									token: Token,
									job: modifyObj.newInput
								},
								success: res => {
									console.log(JSON.stringify(res.data));
									if (res.statusCode == 404) {
										uni.showToast({
											icon: "none",
											title: "修改失败"
										})
									} else if ("error" in res.data) {
										uni.showToast({
											icon: "none",
											title: "修改失败"
										})
									} else {
										uni.showToast({
											icon: "none",
											title: "修改成功"
										})
										this.Info.job = res.data.job;
									}
								}
							});
							break;
						case 7: //邮箱
							if (!(/^[a-zA-Z0-9_-]+@[a-zA-Z0-9_-]+(\.[a-zA-Z0-9_-]+)+$/.test(modifyObj.newInput))) {
								uni.showToast({
									icon: 'error',
									title: '邮箱不符合要求'
								});
							} else {
								uni.request({
									url: 'http://106.14.62.110:8080/user/changeInfo/email',
									method: "POST",
									data: {
										token: Token,
										email: modifyObj.newInput
									},
									success: res => {
										console.log(JSON.stringify(res.data));
										if (res.statusCode == 404) {
											uni.showToast({
												icon: "none",
												title: "修改失败"
											})
										} else if ("error" in res.data) {
											uni.showToast({
												icon: "none",
												title: "修改失败"
											})
										} else {
											uni.showToast({
												icon: "none",
												title: "修改成功"
											})
										}
									}
								});
							}
							break;
					}
				}
			},

			showPasswordPop() { //显示修改密码的弹出层
				this.showPassword = true;
				this.showCommom = false;
			},

			confirm(e) { //修改生日,绑定的是日历里的确定按钮
				var Token;
				uni.getStorage({ //获取login的token
					key: 'login_token',
					success: function(res) {
						Token = res.data;
					}
				});
				uni.request({
					url: 'http://106.14.62.110:8080/user/changeInfo/birthday',
					method: "POST",
					data: {
						token: Token,
						birthday: e["fulldate"]
					},
					success: res => {
						console.log(res.data);
						if (res.statusCode == 404) {
							uni.showToast({
								icon: "error",
								title: "404 not found"
							})
						} else if ("error" in res.data) {
							uni.showToast({
								icon: "error",
								title: "修改失败"
							})
						} else {
							uni.showToast({
								icon: "success",
								title: "修改成功"
							})
							this.Info.birth = res.data.birthday;
							this.Info.age = res.data.age + "";
							this.Info.star = res.data.astro;
						}
					}
				})
			},

			yesPass() { //提交修改密码请求
				// var password = this.password;
				// var passwordAgain = this.passwordAgain;
				// var old = this.oldPassword;
				// 判断密码是否符号要求
				if (this.judgeVoid(this.password)) {
					return;
				} else if (!(/^[A-Za-z\d]{6,16}$/.test(this.password))) {
					uni.showToast({
						icon: 'error',
						title: '密码不符合要求'
					});
				} else {
					var Token;
					uni.getStorage({ //获取login的token
						key: 'login_token',
						success: function(res) {
							Token = res.data;
						}
					});
					uni.request({
						url: 'http://106.14.62.110:8080/user/changeInfo/password',
						method: "POST",
						data: {
							token: Token,
							prePassword: this.oldPassword,
							passwordFir: this.password,
							passwordSec: this.passwordAgain
						},

						success: res => {
							console.log(JSON.stringify(res.data));
							if (res.statusCode == 404) {
								uni.showToast({
									icon: "none",
									title: "修改失败"
								})
							} else if ("inconsistentPasswordError" in res.data) {
								uni.showToast({
									icon: "none",
									title: "两次密码不一致"
								})
							} else if ("prePasswordError" in res.data) {
								uni.showToast({
									icon: "none",
									title: "旧密码错误!"
								})
							} else {
								uni.showToast({
									icon: "none",
									title: "修改成功"
								})
								this.showPassword = false;
							}

						}
					})
				}
			}
		}
	}
</script>

<style>
	.popInput {
		display: flex;
		flex-direction: row;
		margin-top: 10%;
		margin-left: 5%;
		margin-right: 5%;
	}

	.yesOrNot {
		display: flex;
		flex-direction: row;
		margin-top: 10%;
		margin-left: 5%;
		margin-right: 5%;
		margin-bottom: 10%;
	}

	.cancleClass {
		margin-right: 40%;
	}

	.popLayer {
		border-radius: 5%;
	}

	.popSex {
		border-radius: 5%;
		width: 300rpx;
		height: 100rpx;
	}

	text {
		margin-top: 4%;
		margin-right: 5%;
	}

	.popPassword {
		display: flex;
		flex-direction: column;
		margin-top: 5%;
		margin-right: 5%;
		margin-left: 5%;
	}

	.popSex {
		display: flex;
		flex-direction: column;
		margin-top: 5%;
		margin-right: 5%;
		margin-left: 5%;
		width: 100px;
	}

	.inputPassword {
		display: flex;
		flex-direction: row;
		margin: 10rpx;
	}
</style>
