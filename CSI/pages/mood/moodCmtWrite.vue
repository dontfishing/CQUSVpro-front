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
		
		methods :{
			cancel() {
				
			},
			
			publish() {
				
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