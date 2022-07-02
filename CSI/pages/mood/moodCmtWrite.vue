<template>
	<view>
		<!--文本内容-->
		<view>
			<u--textarea class="content" v-model="cmtContent" placeholder="请输入内容" height=200px maxlength=200 count>
			</u--textarea>
		</view>
		<!--按钮区域-->
		<view class="buttons">
			<u-button class="button1" text="取消" type="primary" color="#c4c6c9" @click="cancel()"></u-button>
			<u-button class="button2" text="发表" type="primary" color="#8967D3" @click="publish()"></u-button>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				moodContent: '', //文本内容
			}
		},

		methods: {
			cancel() {
				uni.navigateBack({
					delta: 1
				});
			},

			publish() { //发表，成功后返回与我相关
				if (this.moodContent == "") {
					uni.showToast({
						icon: "error",
						title: '评论内容不能为空!'
					});
				} else {
					let _this = this;
					let text = _this.moodContent;
					let moodId = uni.getStorageSync('moodId');
					let Token = uni.getStorageSync('login_token');
					uni.request({ //请求发表评论
						url: 'http://106.14.62.110:8080/writeComment',
						method: "POST",
						data: {
							token: Token,
							moodId: moodId,
							cmtContent: text,
						},
						success: (res) => {
							if ("commenterName" in res.data) {
								uni.showToast({
									icon: "success",
									title: "发表成功!",
									duration: 1000,
									success: function() {
										setTimeout(function() {
											uni.navigateBack({
												delta: 1
											});
										}, 2000);
									}
								})
							} else {
								uni.showToast({
									icon: "error",
									title: "发表失败"
								})
							}
						}
					});
				}
			},

		}
	}
</script>

<style>
	.content {
		margin-top: 5%;
		margin-left: 2%;
		margin-right: 2%;
	}

	.buttons {
		margin-top: 50%;
		display: flex;
		flex-direction: row;
		margin-left: 10%;
		margin-right: 10%;
	}

	.button1 {
		margin-right: 35%;
	}
</style>
