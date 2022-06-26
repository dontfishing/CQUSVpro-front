<template>
	<view>
		<view class="sliderSet">
			<text>语速</text>
			<slider value="5" min="0" max="15" @change="sliderChangeS" show-value="true"/>
		</view>
		<view class="sliderSet">
			<text>音调</text>
			<slider value="5" min="0" max="15" @change="sliderChangeT" show-value="true"/>
		</view>
		<view class="sliderSet">
			<text>音量</text>
			<slider value="5" min="0" max="9" @change="sliderChangeV" show-value="true"/>
		</view>
		<u-cell-group>
			<u-cell  title="音色选择" isLink @click="voiceChoice()">
				<u-icon slot="icon" size="30" name="volume-fill"></u-icon>
				<!-- <u-badge count="99" :absolute="false" slot="right-icon"></u-badge> -->
			</u-cell>
		</u-cell-group>
		<u-popup :show="show" @close="close" @open="open">
            <view>
				<u-grid
						:border="true"
						@click="click"
				>
					<u-grid-item
							v-for="(baseListItem,baseListIndex) in baseList"
							:key="baseListIndex"
					>
						<u-icon
								:customStyle="{paddingTop:20+'rpx'}"
								:name="baseListItem.name"
								:size="22"
						></u-icon>
						<text class="grid-text">{{baseListItem.title}}</text>
					</u-grid-item>
					<u-toast ref="uToast" />
				</u-grid>
            </view>
		</u-popup>
	</view>
</template>

<script>
	export default {
		data () {
			return {
				show: false,
				 baseList: [{
								name: 'photo',
				                title: '音色1'
				            },
				            {
								name: 'lock',
				                title: '音色2'
				            },
				            {
				                name: 'star',
				                title: '音色3'
				            },
				           ]
			}
		},
		
		methods: {
			sliderChangeS(e) {
				uni.request({
					url: '',
					method:"POST",
					data:{
						"speed": e.detail.value
					}
				})
			},
			
			sliderChangeT(e) {
				uni.request({
					url: '',
					method:"POST",
					data:{
						"tune": e.detail.value
					}	
				})
			},
			
			sliderChangeV(e) {
				uni.request({
					url: '',
					method:"POST",
					data:{
						"volume": e.detail.value
					}	
				})
			},
			
			voiceChoice() {
				this.show = true;
				
			},
			
			open() {
				
			},
			
			close() {
				this.show = false;
			}
		}
		
	}
</script>

<style>
	view {
		margin-top: 5%;
	}
	
	text {
		margin-top: 6%;
		margin-right: 2%;
		margin-left: 2%;
		font-size: 20px;
	}
	
	.sliderSet {
		border-radius: 5%;
		margin-left: 3%;
		margin-right: 3%;
	}
	
	.grid-text {
	    font-size: 14px;
	    color: #909399;
	    padding: 10rpx 0 20rpx 0rpx;
	    /* #ifndef APP-PLUS */
	    box-sizing: border-box;
	    /* #endif */
	}
</style>