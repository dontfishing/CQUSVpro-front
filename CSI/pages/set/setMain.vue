<template>
	<view>
		<view class="information">
			<avatar class="photo" selWidth="200px" selHeight="200px" ref='avatar' fileType='png' @upload="myUpload"
				:avatarSrc="imageURL" avatarStyle="width: 160rpx; height: 160rpx; border-radius: 6rpx;">
			</avatar>
			<!-- <u-image class="photo" width="160rpx" height="160rpx" radius="6rpx" :src="imageURL"></u-image> -->
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
				<u-cell class="cell" v-show="admin" icon="setting" title="管理员页面" url="/pages/set/voiceSet" :isLink="true"></u-cell>
				<u-cell class="cell-end" icon="trash-fill" title="退出登录" @click="changeShowEnd()"></u-cell>
			</u-cell-group>
		</view>
		<view>
			<u-action-sheet :actions="endList" @select="selectClick" :closeOnClickOverlay="true"
				:closeOnClickAction="true" :show="showEnd" cancelText="取消"  @close="showEnd=false">
			</u-action-sheet>
		</view>
	</view>
</template>

<script>
	import avatar from "../../components/yq-avatar/yq-avatar.vue";
	export default {
		data() {
			return {
				admin: false,
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
			var _this =this;
			_this.admin = uni.getStorageSync('admin');
			uni.getStorage({ // 获取缓存中的图片url
				key: 'ImgUrl',
				success: function(getImgRes) {
					_this.imageURL = getImgRes.data;
				},
				fail: () => {
					console.log(JSON.stringify(getImgRes.data));
					console.log('getImg fail');
				}
			});
			uni.getStorage({ // 获取缓存中的token
				key: 'login_token',
				success: function(getTokenRes) {
					_this.token = getTokenRes.data;
				},
				fail: () => {
					console.log(JSON.stringify(getTokenRes.data));
					console.log('getstorage fail');
				}
			});
			uni.getStorage({
				key: 'user_Name',
				success: function(res) {
					_this.userName = res.data;
				}
			});
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
			myUpload(rsp) {	//点击头像更改并上传
				let _this = this;
				_this.imageURL = rsp.path;
				let Token = uni.getStorageSync('login_token');
				uni.uploadFile({
					url: 'http://106.14.62.110:8080/uploadImg', //仅为示例，非真实的接口地址
					filePath: _this.imageURL,
					name: "avator",
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
			},
			gotoPhoto() { //跳转到更改头像
			let avatar = this.$refs.avatar;
			avatar.fChooseImg(1, {selWidth: "200px", selHeight: "200px", expWidth: "160rpx", expHeight: "160rpx", inner:false});
			},
			changeShowEnd() { //显示退出登录菜单
				this.showEnd = true;
			},
			selectClick(obj) { //操作退出菜单
				if (obj.id == 1) { //退出登录
					uni.clearStorage();
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
		},
		components: {
			avatar
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
