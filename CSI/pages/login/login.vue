<template>
	<view>
		<view class="title">
			<text class="welcome">Welcome</text></br>
			<text class="voice">欢迎来到语音心情吧</text>
		</view>
		<view class="Input">
			<u-icon name="account"></u-icon>
			<u--input placeholder="账号" border="surround" v-model="account" clearable></u--input>
		</view>
		<view class="Input">
			<u-icon name="lock"></u-icon>
			<u--input placeholder="密码" type="password" border="surround" v-model="password" clearable></u--input>
		</view>
		<view style="margin-top: 80px; width: 60%;" class="uButton">
			<u-button type="primary" text="登录" shape="circle" color="#8967D3" @click="userLogin()"></u-button>
		</view>
		<view class="reg">
			<view>
				<u-button class="uReg" type="primary" text="注册" color="#8967D3" :plain="true" shape="circle" size="mini"
					@click="reg()"></u-button>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				account: "",
				password: "",
				value: false,
				remember: "0"
			}
		},
		onBackPress(options) { //禁用返回
			if (options.from == 'backbutton') {
				return false;
			} else if (options.from == 'navigateBack') {
				return false;
			}
		},
		methods: {
			userLogin() {
				uni.request({
					url: 'http://106.14.62.110:8080/userLogin', //api地址
					method: "POST",
					data: {
						account: this.account,
						password: this.password,
						remember: this.remember
					},

					success: res => {
						if (res.statusCode == 404) { //返回的状态码
							uni.showToast({
								icon: 'none',
								title: '密码或用户名错误',
							});
							//return;
						} else if ("error" in res.data && res.data["error"] == "can not found") {
							uni.showToast({
								icon: 'none',
								title: '密码或用户名错误',
							});
							//return;
						} else {
							uni.showToast({
								icon: 'none',
								title: '登录成功'
							});
							uni.setStorage({
								key: 'login_token',
								data: res.data.token
							});
							uni.setStorage({
								key: 'user_Name',
								data: res.data.userName
							});
							uni.setStorage({
								key: 'admin',
								data: res.data.admin
							});
							uni.setStorage({
								key: 'voice_setting',
								data: {
									ttsSpd: 5,
									ttsPit: 5,
									ttsVol: 5,
									ttsPer: 4
								}
							});
							uni.setStorage({
								key: 'ImgUrl',
								data: res.data.userImg
							})
							uni.reLaunch({
								url: '/pages/passage/passageMain'
							});
						}
					},

					fail: () => {},

					complete: () => {}
				});
			},

			reg() {
				uni.navigateTo({
					url: 'logon'
				})
			}
		}
	}
</script>

<style>
	/* 	page {
		background-color: #f5f6f7;
	} */

	.allInput {
		display: flex;
		flex-direction: column;
		margin: 20%;
		justify-content: center;
		align-items: center;
	}

	.title {
		margin-top: 80rpx;
		margin-left: 40rpx;
		font-family: Harrington;
	}

	.welcome {
		font-size: 50px;
		color: #212121;
		font-weight: bold;
	}

	.voice {
		font-size: 20px;
		color: #212121;
	}

	.Input {
		display: flex;
		flex-direction: row;
		flex-wrap: nowrap;
		margin-top: 10%;
		margin-left: 8%;
		margin-right: 8%;
	}

	.uButton {
		margin-left: 20%;
	}

	.reg {
		margin-top: 8%;
		margin-left: 30%;
		margin-right: 30%;
	}

	.uReg {
		width: 40%;
	}
</style>
