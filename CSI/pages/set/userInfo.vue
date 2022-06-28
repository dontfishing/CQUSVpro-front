<template>
	<view>
		<u-cell-group>
			<!--界面单元格-->
			<u-cell title="用户名" isLink @click="pop(0)">
				<u-icon slot="icon" size="32" name="account-fill"></u-icon>
			</u-cell>
			<u-cell title="性别" isLink @click="showSexPop(1)">
				<u-icon slot="icon" size="32" name="man"></u-icon>
			</u-cell>
			<u-cell title="年龄" isLink @click="pop(2)">
				<u-icon slot="icon" size="32" name="calendar"></u-icon>
			</u-cell>
			<u-cell title="邮箱" isLink @click="pop(3)">
				<u-icon slot="icon" size="32" name="email-fill"></u-icon>
			</u-cell>
			<u-cell title="密码" isLink @click="showPasswordPop()">
				<u-icon slot="icon" size="32" name="lock-fill"></u-icon>
			</u-cell>
		</u-cell-group>
		<view class="popLayer">
			<!--除密码和性别外其余单元格的弹出层-->
			<!--用showCommon控制是否弹出-->
			<u-popup :show="showCommom" @close="close" @open="open" mode="center">
				<view class="popInput">
					<!--指示和输入-->
					<text>{{modifyValue[popNum].newInfo}}</text>
					<u-input v-model="modifyValue[popNum].newInput"></u-input>
				</view>
				<!--取消和确认按钮-->
				<view class="yesOrNot">
					<u-button class="cancleClass" type="primary" text="取消" shape="circle" @click="cancle()"
						size="small">
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
			<u-popup :show="showSex" @close="close" @open="open" mode="center">
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
						size="small">
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
			<u-popup :show="showPassword" @close="close" @open="open" mode="center">
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
							size="small">
						</u-button>
						<u-button class="yesClass" type="primary" text="确定" shape="circle" @click="yesPass()"
							size="small">
						</u-button>
					</view>
				</view>
			</u-popup>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				token: "",
				showCommom: false, //除密码和性别外的弹出层控制
				showSex: false,
				showPassword: false, //密码的弹出层控制
				popNum: 0, //索引
				newName: "", //新用户名
				newGender: "", //新性别
				newAge: "", //新年龄
				newMail: "", //新邮箱地址
				oldPassword: "", //旧密码
				password: "", //输入的新密码
				passwordAgain: "", //再次输入的新密码
				genderList: [{
					name: '女',
					disabled: true
				}, {
					name: '男',
					disabled: false
				}], //性别列表
				modifyValue: [{ //输入绑定的对象
					newInfo: "新用户名",
					newInput: ""
				}, {
					newInfo: "性别",
					newInput: ""
				}, {
					newInfo: "年龄",
					newInput: ""
				}, {
					newInfo: "邮箱",
					newInput: ""
				}]
			}
		},

		methods: {
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

			open() {

			},

			cancle() { //直接关闭弹出层
				this.showCommom = false;
				this.showPassword = false;
				this.showSex = false;
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
							method:"POST",
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
								}

							}
						});
						break;
					case 2: //年龄
						uni.request({
							url: 'http://106.14.62.110:8080/user/changeInfo/age',
							method:"POST",
							data: {
								token: Token,
								age: modifyObj.newInput
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
						break;
					case 3:
						uni.request({
							url: 'http://106.14.62.110:8080/user/changeInfo/email',
							method:"POST",
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
						break;
				}
			},

			showPasswordPop() {
				this.showPassword = true;
				this.showCommom = false;
			},

			yesPass() { //提交修改密码请求
				var password = this.password;
				var passwordAgain = this.passwordAgain;
				var old = this.oldPassword;
				// if (password != passwordAgain) {
				// 	uni.showToast({
				// 		icon:"error",
				// 		title: '密码输入要一致!'
				// 	})
				// }
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
						prePassword: old,
						passwordFir: password,
						passwordSec: passwordAgain
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
						}

					}
				})
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
