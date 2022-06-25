<template>
	<view>
		<u-cell-group> <!--界面单元格-->
			<u-cell  title="用户名" isLink @click="pop(0)">
				<u-icon slot="icon" size="32" name="account-fill"></u-icon>
			</u-cell>
			<u-cell title="性别" isLink @click="pop(1)">
				<u-icon slot="icon" size="32" name="man"></u-icon>
			</u-cell>
			<u-cell title="年龄" isLink @click="pop(2)">
				<u-icon slot="icon" size="32" name="calendar"></u-icon>
			</u-cell>
			<u-cell title="邮箱" isLink @click="pop(3)">
				<u-icon slot="icon" size="32" name="email-fill"></u-icon>
			</u-cell>
			<u-cell title="密码" isLink @click="modify()">
				<u-icon slot="icon" size="32" name="lock-fill"></u-icon>
			</u-cell>
		</u-cell-group>
		<view class="popLayer"> <!--除密码外其余单元格的弹出层-->
			<!--用showCommon控制是否弹出-->
			<u-popup :show="showCommom" @close="close" @open="open" mode="center">
				<view class="popInput"> <!--指示和输入-->
					<text>{{modifyValue[popNum].newInfo}}</text>
					<u-input v-model="modifyValue[popNum]"></u-input>
				</view>
				<!--取消和确认按钮-->
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
						@click="yes(modifyValue[popNum])" 
						size="small">
					</u-button>
				</view>
			</u-popup>
		</view>
		<view class="popLayer"> <!--=密码单元格的弹出层-->
			<!--用showPassword控制是否弹出-->
			<u-popup :show="showPassword" @close="close" @open="open" mode="center">
				<view class="popPassword">
					<view class="inputPassword">
						<text>旧密码</text>
						<u-input></u-input>
					</view>
					<view class="inputPassword">
						<text>新密码</text>
						<u-input v-model="newPassword"></u-input>
					</view>
					<view class="inputPassword">
						<text>确认密码</text>
						<u-input v-model="newPasswordAgain"></u-input>
					</view>
					<!--取消和确认按钮-->
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
							@click="yesPass()" 
							size="small">
						</u-button>
					</view>
				</view>
			</u-popup>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				showCommom : false, //除密码外的弹出层控制
				showPassword : false, //密码的弹出层控制
				popNum : 0, //索引
				newName : "", //新用户名
				newGender : "" , //新性别
				newAge : "", //新年龄
				newMail : "" , //新邮箱地址
				newPassword : "", //新密码
				password : "", //输入的新密码
				passwordAgain : "", //再次输入的新密码
				modifyValue : [{ //输入绑定的对象
					newInfo : "新用户",
					newInput : ""
				},{
					newInfo : "性别",
					newInput : ""
				},{
					newInfo : "年龄",
					newInput : ""
				},{
					newInfo : "邮箱",
					newInput : ""
				}]
			}
		},
		
		methods : {
			pop(num) { //弹出层信息根据传入的参数改变
				this.showCommom = true
				this.showPassword = false
				this.popNum = num
			},
			
			close() { //直接关闭弹出层
				this.showCommom = false
				this.showPassword = false
			},
			
			open() {
				
			},
			
			cancle() { //直接关闭弹出层
				this.showCommom = false
				this.showPassword = false
			},
			
			yes(modifyObj) { //点击确认，传递参数
				this.showCommom = false
				this.showPassword = false
				
				switch(popNum) { //根据popNum判断修改的哪个信息
					case 0 :
						uni.request({
							url: '',
							method:"POST",
							data : {
								newName : modifyObj.newInput
							},
							success: res => {
								if(res.statusCode == 404) {
									uni.showToast({
										icon:"none",
										title:"修改失败"
									})
								}
								
								uni.showToast({
									icon:"none",
									title:"修改成功"
								})
								
							}
						})
					case 1 :
						uni.request({
							url: '',
							data : {
								newGender : modifyObj.newInput
							},
							success: res => {
								if(res.statusCode == 404) {
									uni.showToast({
										icon:"none",
										title:"修改失败"
									})
								}
								
								uni.showToast({
									icon:"none",
									title:"修改成功"
								})
								
							}
						})
					case 2 :
						uni.request({
							url: '',
							data : {
								newAge : modifyObj.newInput
							},
							success: res => {
								if(res.statusCode == 404) {
									uni.showToast({
										icon:"none",
										title:"修改失败"
									})
								}
								
								uni.showToast({
									icon:"none",
									title:"修改成功"
								})
								
							}
						})
					case 3 : 
						uni.request({
							url: '',
							data : {
								newMail : modifyObj.newInput
							},
							success: res => {
								if(res.statusCode == 404) {
									uni.showToast({
										icon:"none",
										title:"修改失败"
									})
								}
								
								uni.showToast({
									icon:"none",
									title:"修改成功"
								})
								
							}
						})
				}
			},
			
			modify() {
				this.showPassword = true
				this.showCommom = false
			},
			
			yesPass() {
				if(password == passwordAgain) {
					uni.request({
						url:'',
						data: {
							newPassword : password
						}
					})
				}
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
		flex-direction: column;
		margin-top: 5%;
		margin-right: 5%;
		margin-left: 5%;
	}
	
	.inputPassword {
		display: flex;
		flex-direction: row;
	}
</style>