<template>
	<view>
		<view class="information">
			<u-image class="photo" width="160rpx" height="160rpx" radius="6rpx" :src="imageURL"></u-image>
			<view class="userInfo">
				<view class="userName">
					{{userName}}
				</view>
				<u-button color="#8967D3" shape="circle" class="button" text="修改头像" type="primary" size="mini"
					@click="gotoPhoto()">
				</u-button>
			</view>
		</view>
		<view>
			<u-cell-group>
				<u-cell class="cell" icon="setting-fill" title="个人信息" url="/pages/set/userInfo" :isLink="true"></u-cell>
				<u-cell class="cell" icon="volume-fill" title="语音设置" url="/pages/set/voiceSet" :isLink="true"></u-cell>
				<u-cell class="cell-end" icon="trash-fill" title="退出登录" @click="changeShowEnd()"></u-cell>
			</u-cell-group>
		</view>
		<view>
			<u-action-sheet :actions="endList" @select="selectClick" :closeOnClickOverlay="true"
				:closeOnClickAction="true" :show="showEnd" cancelText="取消">
			</u-action-sheet>
		</view>
	</view>
</template>

<script>
	var that = this;
	export default {
		data() {
			return {
				imageURL: '', //头像
				userName: "", //用户名
				showEnd: false, //管理退出登录
				endList: [{ //退出登录后的操作菜单
						name: '退出登录',
						id: 1
					},
					{
						name: '关闭应用',
						id: 2
					}
				],
				token: '',

			};
		},
		onLoad() { //每次加载都会重新刷新
			var imgUrl;
			var name;
			uni.getStorage({ // 获取缓存中的图片url
				key: 'ImgUrl',
				success: function(getImgRes) {
					imgUrl = getImgRes.data;
				},
				fail: () => {
					console.log(JSON.stringify(getImgRes.data));
					console.log('getImg fail');
				}
			});
			this.imageURL = imgUrl;
			var Token;
			uni.getStorage({ // 获取缓存中的token
				key: 'login_token',
				success: function(getTokenRes) {
					Token = getTokenRes.data;
				},
				fail: () => {
					console.log(JSON.stringify(getTokenRes.data));
					console.log('getstorage fail');
				}
			});
			uni.getStorage({
				key: 'user_Name',
				success: function(res) {
					name = res.data;
					console.log(name);
				}
			});
			this.userName = name;
		},
		methods: {
			storeImg(img_url) { //头像图片存入缓存
				uni.setStorage({
					key: 'ImgUrl',
					data: img_url,
					success: function() {
						console.log(img_url);
					}
				});
			},
			gotoPhoto() { //跳转到更改头像
				let _this = this;
				var Token;
				uni.getStorage({ // 获取缓存中的token
					key: 'login_token',
					success: (getTokenRes) => {
						Token = getTokenRes.data;
					},
					fail: () => {
						console.log(JSON.stringify(getTokenRes.data));
						console.log('getstorage fail');
					}
				});
				uni.chooseImage({ // 选择图片
					count: 1,
					sizeType: ['compressed'],
					sourceType: ['album'], //album是打开手机相册
					success: (chooseRes) => { // 选择图片成功
						const tmpChoose = chooseRes.tempFilePaths;
						uni.uploadFile({
							url: 'http://106.14.62.110:8080/uploadImg', //仅为示例，非真实的接口地址
							filePath: tmpChoose[0],
							fileType: "image",
							name: "image",
							success: (uploadFileRes) => {
								let imgURL = JSON.parse(uploadFileRes.data);
								uni.request({ // 传url和token给后端
									url: 'http://106.14.62.110:8080/confirmImg', //api地址
									method: "POST",
									data: {
										url: imgURL.img_url,
										token: Token
									},
									success: (res) => {
										console.log(JSON.stringify(res.data));
										if (res.statusCode == 404) { //返回的状态码
											uni.showToast({
												icon: 'error',
												title: '网页失踪了',
											});
										} else {
											uni.setStorage({
												key: 'ImgUrl',
												data: imgURL.img_url
											})
											_this.imageURL = imgURL.img_url;
										}
									},
									fail: () => {},
									complete: () => {}
								});
							}
						});
					}
				});
			},
			changeShowEnd() { //显示退出登录菜单
				this.showEnd = true;
			},
			selectClick(obj) { //操作退出菜单
				if (obj.id == 1) { //退出登录
					uni.navigateTo({
						url: '/pages/login/login'
					});
				} else { //退出应用
					switch (uni.getSystemInfoSync().platform) {
						case 'android':
							plus.runtime.quit();
							break;
						case 'ios':
							plus.ios.import('UIApplication').sharedApplication().performSelector('exit');
							break;
					};
				}
			},
		}
	}
</script>

<style>
	page {
		background: #EDEDED;
	}

	.information {
		//用户信息部分
		display: flex;
		height: 260rpx;
		background-color: #f8f8f8;
	}

	.photo {
		//头像
		margin: 50rpx;
		margin-left: 97rpx;
	}

	.userInfo {
		//用户名和修改头像
		display: flex;
		flex-direction: column;
		margin-left: 70rpx;
		margin-top: 65rpx;
	}

	.userName {
		//用户名
		font-size: larger;
		font-weight: bold;
	}

	.button {
		//修改头像按钮
		margin-top: 20rpx;

	}

	.cell {
		//设置的单元
		margin-top: 20rpx;
		background-color: #f8f8ff;
	}

	.cell-end {
		//退出的单元
		margin-top: 50rpx;
		background-color: #f8f8ff;
	}
</style>
