<template>
	<view>
		<!--文本内容-->
		<view>
			<u--textarea class="content" v-model="moodContent" placeholder="请输入内容" height=200px maxlength=200 count>
			</u--textarea>
		</view>
		<view class="photoAndTags">
			<!--发布照片-->
			<view class="imgLoad">
				<u-avatar shape="square" size="150" bg-color="#f3f4f6" @click="chooseImg()" color="black"
					:src="src">
				</u-avatar>
			</view>
			<!--标签区域-->
			<view class="tags">
				<u-tag :text="emoType" size="mini" closable :show="close1" @close="closeTag1()" plain borderColor="#909399" color="#909399" ></u-tag>
				<u-tag :text="faceType" size="mini" closable :show="close2" @close="closeTag2()" plain borderColor="#909399" color="#909399" ></u-tag>
				<u-tag :text="beauty" size="mini" closable :show="close3" @close="closeTag3()" plain borderColor="#909399" color="#909399" ></u-tag>
				<u-tag :text="age" size="mini" closable :show="close4" @close="closeTag4()" plain borderColor="#909399" color="#909399" ></u-tag>
				<u-tag :text="emoTag" size="mini" closable :show="close5" @close="closeTag5()" plain borderColor="#909399" color="#909399" ></u-tag>
				<u-tag text="life" size="mini" :show="close6" plain borderColor="#909399" color="#909399" ></u-tag>
			</view>
		</view>
		<!--取消和确认按钮-->
		<view class="buttons">
			<u-button class="button1" text="取消" type="primary" color="#c4c6c9" @click="cancel()"></u-button>
			<u-button class="button2" text="发表" type="rimary" color="#8967D3" @click="publish()"></u-button>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				moodContent: '', //文本内容
				text: '发布照片', //发布照片文字
				src: '', //照片来源
				close1: false,
				close2: false,
				close3: false,
				close4: false,
				close5: false,
				close6: false,
				emoType: '', //心情类型
				faceType: '', //脸型
				beauty: '', //颜值
				age: '', //年龄
				emoTag: '', //心情标签
				picUrl: '' ,//上传图片返回的url
				count: 0, //记录删除的标签数量
				iniBeauty: 1.5,
				iniAge: 0,
			}
		},

		methods: {
			chooseImg() { //选择照片
				let _this = this;
				uni.chooseImage({
					count: 1, //只能选择一张图片
					sizeType: ['compressed'], //压缩
					sourceType: ['album','camera'], //从相册选择
					success: (res) => {
						const tmpChoose = res.tempFilePaths;
						uni.uploadFile({ //上传照片
							url: 'http://106.14.62.110:8080/recognizeFace',
							filePath: tmpChoose[0],
							fileType: "image",
							name: "image",
							success: (uploadFileRes) => {
								let tmp=JSON.parse(uploadFileRes.data);
								_this.emoType = tmp.EMOTION_TYPE;
								_this.faceType = tmp.FACE_TYPE;
								_this.beauty = "颜值:" + tmp.BEAUTY + "";
								_this.age = "年龄:" + tmp.AGE + "";
								_this.emoTag = tmp.EMO_TAG;
								_this.src = tmp.url;
								_this.picUrl = tmp.url;
								_this.iniBeauty = tmp.BEAUTY;
								_this.iniAge = tmp.AGE;
								_this.close1 = true;
								_this.close2 = true;
								_this.close3 = true;
								_this.close4 = true;
								_this.close5 = true;
							}
						})	
					}

				})
			},
			
			cancel() { //取消，跳转到与我相关
				uni.navigateTo({
					url:'/pages/myself/myselfMain'
				})
			},
			
			publish() { //发表
				let _this = this;
				let Token = uni.getStorageSync('login_token');
				let tmpPicUrl = _this.picUrl;
				let tmpMoodContent = _this.moodContent;
				let tmpEmoType = _this.emoType;
				let tmpFaceType = _this.faceType;
				let tmpBeauty = _this.iniBeauty;
				let tmpAge = _this.iniAge;
				let tmpEmoTag = _this.emoTag;
				console.log(Token);
				uni.request({
					url:'http://106.14.62.110:8080/moodPublish',
					method:"POST",
					data: {
						token: Token,
						url: tmpPicUrl,
						moodContent: tmpMoodContent,
						EMOTION_TYPE: tmpEmoType,
						FACE_TYPE: tmpFaceType,
						BEAUTY: tmpBeauty,
						AGE: tmpAge,
						EMO_TAG: tmpEmoTag
					},
					success: res => {
						console.log(res.data);
						if(res.statusCode == 404) {
							uni.showToast({
								icon:"none",
								title: "404 not found"
							})
						} else if("info" in res.data && res.data.info == "failed") {
							uni.showToast({
								icon:"none",
								title:"发表失败"
							})
						} else {
							uni.showToast({
								icon:"none",
								title:"发表成功",
								duration: 1000,
								success: function() {
									setTimeout(function() {
										uni.redirectTo({
											url: "../myself/myselfMain"
										})
									}, 2000);
								}
							})
						}
					}
				})
			},
			
			closeTag1() { //删除标签
				let _this = this;
				_this.close1 = false;
				_this.count ++;
				if(_this.count == 5){
					_this.close6 = true;
				}
				_this.emoType = "";
			},
				
			closeTag2() { //删除标签
				let _this = this;
				_this.close2 = false;
				_this.count ++;
				if(_this.count == 5){
					_this.close6 = true;
				}
				_this.faceType = "";
			},

			closeTag3() { //删除标签
				let _this = this;
				_this.close3 = false;
				_this.count ++;
				if(_this.count == 5){
					_this.close6 = true;
				}
				_this.iniBeauty = -1; 
			},
				
			closeTag4() { //删除标签
				let _this = this;
				_this.close4 = false;
				_this.count ++;
				if(_this.count == 5){
					_this.close6 = true;
				}
				_this.iniAge = -1;
			},

			closeTag5() { //删除标签
				let _this = this;
				_this.close5 = false;
				_this.count ++;
				if(_this.count == 5){
					_this.close6 = true;
				}
				_this.emoTag = "";
			}
		}
	}
</script>

<style>
	.content {
		margin-top: 5%;
		margin-left: 2%;
		margin-right: 2%;
	}

	.imgLoad {
		margin-top: 10%;
		margin-left: 5%;
		margin-right: 5%;
	}
	
	.buttons {
		margin-top: 30%;
		display: flex;
		flex-direction: row;
		margin-left: 10%;
		margin-right: 10%;
	}
	
	.button1 {
		margin-right: 35%;
	}
	
	.photoAndTags {
		display: flex;
		flex-direction: row;
	}
	
	.tags {
		display: flex;
		flex-direction: row;
		flex-wrap: wrap;
		margin-top: 10%;
	}
	
	.miniTag {
		margin: 1%;
	}
</style>
