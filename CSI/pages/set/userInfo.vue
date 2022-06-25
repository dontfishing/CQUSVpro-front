<template>
	<view>
		<u-cell-group>
			<u-cell  title="用户名" isLink @click="pop('新用户')">
				<u-icon slot="icon" size="32" name="account-fill"></u-icon>
			</u-cell>
			<u-cell title="性别" isLink @click="pop('修改性别')">
				<u-icon slot="icon" size="32" name="man"></u-icon>
			</u-cell>
			<u-cell title="年龄" isLink @click="pop('年龄')">
				<u-icon slot="icon" size="32" name="calendar"></u-icon>
			</u-cell>
			<u-cell title="邮箱" isLink @click="pop('邮箱地址')">
				<u-icon slot="icon" size="32" name="email-fill"></u-icon>
			</u-cell>
			<u-cell title="密码" isLink @click="modify()">
				<u-icon slot="icon" size="32" name="lock-fill"></u-icon>
			</u-cell>
		</u-cell-group>
		<view class="popLayer">
			<u-popup :show="showCommom" @close="close" @open="open" mode="center">
				<view class="popInput">
					<text>{{message}}</text>
					<u-input></u-input>
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
			<u-popup :show="showPassword" @close="close" @open="open" mode="center">
				<view class="popPassword">
					<view class="valPassword">
						<text>旧密码</text></br>
						<text>新密码</text></br>
						<text>确认密码</text></br>
					</view>
					<view class="inputPassword">
						<u-input></u-input>
						<u-input></u-input>
						<u-input></u-input>
					</view>
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
				showCommom : false,
				message : "",
				showPassword : false
			}
		},
		
		methods : {
			pop(introduction) {
				this.showCommom = true
				this.message = introduction
				this.showPassword = false
			},
			
			close() {
				this.showCommom = false
				this.showPassword = false
			},
			
			open() {
				
			},
			
			cancle() {
				this.showCommom = false
				this.showPassword = false
			},
			
			yes() {
				this.showCommom = false
				this.showPassword = false
				uni.request({
					
				})
			},
			
			modify() {
				this.showPassword = true
				this.showCommom = false
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
	
	.popPassword {
		display: flex;
		flex-direction: row;
		margin-top: 5%;
		margin-right: 5%;
		margin-left: 5%;
	}
	
	.valPassword {
		
	}
	
	.inputPassword {
		
	}
</style>