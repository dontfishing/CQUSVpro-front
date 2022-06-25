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
				imageURL: 'https://cdn.uviewui.com/uview/album/1.jpg', //头像
				userName: "YK Chen", //用户名
				showEnd: false, //管理退出登录
				endList: [{ //退出登录后的操作菜单
						name: '退出登录',
						id: 1
					},
					{
						name: '关闭应用',
						id: 2
					}
				]
			};
		},
		methods: {

			gotoPhoto() { //跳转到更改头像
			},
			changeShowEnd() {	//显示退出登录菜单
				this.showEnd = true;
			},
			selectClick(obj) {	//操作退出菜单
				if (obj.id == 1) {	//退出登录
					uni.navigateTo({
						url: '/pages/login/login'
					});
				} else {	//退出应用
					switch (uni.getSystemInfoSync().platform) {
						case 'android':
							plus.runtime.quit();
							break;
						case 'ios':
							plus.ios.import('UIApplication').sharedApplication().performSelector('exit');
							break;
					};
				}
			}
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

	.button {	//修改头像按钮
		margin-top: 20rpx;

	}

	.cell {	//设置的单元
		margin-top: 20rpx;
		background-color: #f8f8ff;
	}

	.cell-end {	//退出的单元
		margin-top: 50rpx;
		background-color: #f8f8ff;
	}
</style>
