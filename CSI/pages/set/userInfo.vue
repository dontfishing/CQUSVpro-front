<template>
	<view>
		<u-cell-group>
			<u-cell  title="用户名" isLink @click="userName()">
				<u-icon slot="icon" size="32" name="account-fill"></u-icon>
			</u-cell>
			<u-cell title="性别" isLink @click="gender()">
				<u-icon slot="icon" size="32" name="man"></u-icon>
			</u-cell>
			<u-cell title="年龄" isLink @click="age()">
				<u-icon slot="icon" size="32" name="calendar"></u-icon>
			</u-cell>
			<u-cell title="邮箱" isLink @click="mailbox()">
				<u-icon slot="icon" size="32" name="email-fill"></u-icon>
			</u-cell>
			<u-cell title="密码" isLink @click="password()">
				<u-icon slot="icon" size="32" name="lock-fill"></u-icon>
			</u-cell>
		</u-cell-group>
		<view class="popLayer">
			<u-popup :show="show" @close="close" @open="open" mode="center">
				<view class="popInput">
					<text>新用户</text>
					<u-input placeholder="请输入新用户名"></u-input>
				</view>
				<view class="yesOrNot">
					<u-button
						class="cancleClass" 
						type="primary" 
						text="取消" 
						shape="circle" 
						@click="cancle()"
						size="small">				
					</u-button>
					<u-button
				 		class="yesClass" 
						type="primary" 
						text="确定" 
						shape="circle" 
						@click="yes()" 
						size="small">
					</u-button>
				</view>
			</u-popup>
		</view>
		<view class="popLayer">
			<u-popup :show="show" @close="close" @open="open" mode="center">
				<view class="popInput">
					<text>修改性别</text>
					<u-input placeholder="请输入您的性别"></u-input>
				</view>
				<view class="yesOrNot">
					<u-button
						class="cancleClass" 
						type="primary" 
						text="取消" 
						shape="circle" 
						@click="cancle()"
						size="small">				
					</u-button>
					<u-button
				 		class="yesClass" 
						type="primary" 
						text="确定" 
						shape="circle" 
						@click="yes()" 
						size="small">
					</u-button>
				</view>
			</u-popup>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				show : false
			}
		},
		
		methods : {
			userName() {
				this.show = true
			},
			
			close() {
				this.show = false
			},
			
			open() {
				
			},
			
			cancle() {
				this.show = false
			},
			
			yes() {
				this.show = false
				uni.request({
					
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
	
	text {
		margin-top: 4%;
		margin-right: 5%;
	}
</style>