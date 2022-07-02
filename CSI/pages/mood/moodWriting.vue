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
				<u-avatar :text="text" shape="square" size="150" bg-color="#f3f4f6" @click="chooseImg()" color="black"
					:src="src">
				</u-avatar>
			</view>
			<!--标签区域-->
			<view class="tags">
				<u-tag class="miniTag" v-for="(item, index) in tagList" :key="index" :text="tagList[index]"
				closable :show="close1[index]" @close="close(index)" plain borderColor="#909399" color="#909399" v-if="close2[index]"></u-tag>
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
				tagList:[], //标签列表
				close1: [true,true,true,true,true] ,//标签可删除	
				emoType: '', //心情类型
				faceType: '', //脸型
				beauty: '', //颜值
				age: '', //年龄
				emoTag: '', //心情标签
				picUrl: '' ,//上传图片返回的url
				count: 0, //记录删除的标签数量
				close2: [true,true,true,true,true],
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
							header: {
								
							},
							success: (uploadFileRes) => {
								let tmp=JSON.parse(uploadFileRes.data);
								console.log(tmp.url);
								_this.emoType = tmp.EMOTION_TYPE;
								_this.faceType = tmp.FACE_TYPE;
								_this.beauty = "颜值:" + tmp.BEAUTY + "";
								_this.age = "年龄:" + tmp.AGE + "";
								_this.emoTag = tmp.EMO_TAG;
								_this.src = tmp.url;
								_this.iniBeauty = tmp.BEAUTY;
								_this.iniAge = tmp.AGE;
								_this.tagList.push(_this.emoType);
								_this.tagList.push(_this.faceType);
								_this.tagList.push(_this.beauty);
								_this.tagList.push(_this.age);
								_this.tagList.push(_this.emoTag);
								console.log(_this.src)
							}
						})	
					}

				})
				console.log(_this.src);
			},
			
			cancel() { //取消，跳转到与我相关
				uni.navigateTo({
					url:'/pages/myself/myselfMain'
				})
			},
			
			publish() { //发表
				let _this = this;
				let Token = uni.getStorageSync('login_token');
				console.log(Token);
				console.log(_this.iniAge);
				console.log(_this.iniBeauty);
				uni.request({
					url:'http://106.14.62.110:8080/moodPublish',
					method:"POST",
					data: {
						token: Token,
						url: _this.picUrl,
						moodContent: _this.moodContent,
						EMOTION_TYPE: _this.emoType,
						FACE_TYPE: _this.faceType,
						BEAUTY: _this.iniBeauty,
						AGE: _this.iniAge,
						EMO_TAG: _this.emoTag
					},
					success: res => {
						console.log(res.data);
						if(res.statusCode == 404) {
							uni.showToast({
								icon:"none",
								title: "404 not found"
							})
						} else if("info" in res.data && rea.data.info == "failed") {
							uni.showToast({
								icon:"none",
								title:"发表失败"
							})
						} else {
							uni.showToast({
								icon:"none",
								title:"发表成功"
							})
							// uni.navigateTo({
							// 	url:
							// })
						}
					}
				})
			},
			
			close(index) { //删除标签
				let _this = this;
				_this.close1[index] = false;
				_this.close2[index] = false;
				_this.tagList[index] = ""; //删除后变成空字符串
				_this.count ++;
				console.log(_this.count);
				if(_this.count == 5){
					_this.tagList.push("life");
				}
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
