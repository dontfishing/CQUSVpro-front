<template>
	<view>
		<view class="title">
			<text class="welcome">Welcome</text></br>
			<text class="voice">欢迎来到语音技术吧</text>
		</view>
		<view class="Input">
			<u-icon name="account"></u-icon>
			<u--input placeholder="账号" border="surround" v-model="account" clearable></u--input>
		</view>
		<view class="Input">
			<u-icon name="lock"></u-icon>
			<u--input placeholder="密码" type="password" border="surround" v-model="password" clearable></u--input>
		</view>
		<view class="remReg">
			<view class="rem">
				<text>记住密码</text>
				<u-switch v-model="value" @change="change"></u-switch>
			</view>
			<view>
				<u-button class="uReg" type="primary" text="注册" color="#8967D3" :plain="true" shape="circle" size="mini" @click="reg()"></u-button>
			</view>
		</view>
		<view style="padding-top: 20px; width: 30%;" class="uButton">
			<u-button type="primary" text="登录" shape="circle" color="#8967D3" @click="userLogin()"></u-button>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				account: '',
				password: '',
			 value: false
			}
		},

		methods: {
			userLogin() {
				const data = {
					account: this.account,
					password: this.password
				};

				uni.request({
					url: 'http://127.0.0.1:4523/m1/1152037-0-default/userLogin', //api地址，要改
					method: "POST",
					data: {
						account: this.account,
						password: this.password
					},
					success: res => {
						if (res.statusCode == 404) { //返回的状态码
							uni.showToast({
								icon: 'none',
								title: '密码或用户名错误',
							});
							return;
						}

						uni.showToast({
							icon: 'none',
							title: '登录成功'
						});

						uni.reLaunch({
							url: '/pages/set/setMain'
						});
					},

					fail: () => {

					},

					complete: () => {

					}
				});
			},

			change(e) {
				if (e) {
					uni.setStorage({
						key: "remPassword",
						data: this.password,
						success: function() {
							console.log("success!");
						}
					})
				}
			},

			reg() {
				uni.navigateTo({
					url: 'logon'
				})
			}
		}
	}
</script>

<style lang="scss">
	page {
		background-color: #f5f6f7;
	}

	.allInput {
		display: flex;
		flex-direction: column;
		margin: 20%;
		justify-content: center;
		align-items: center;
	}

	.welcome {
		font-size: 30px;
		color: #212121;
		font-weight: bold;
	}

	.voice {
		font-size: 15px;
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
		margin-left: 35%;
	}

	.remReg {
		margin-top: 5%;
		display: flex;
		flex-direction: row;
	}

	.rem {
		display: flex;
		flex-direction: row;
		margin-left: 8%;
		margin-right: 43%;
	}

	.title {
		margin-top: 80rpx;
		margin-left: 40rpx;
	}
	
	.uReg{
		margin-right: 8%;
	}
</style>
