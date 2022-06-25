<template>
	<view>
		<view class="sliderSet">
			<text>语速</text>
			<slider value="5" min="0" max="15" @change="sliderChange" show-value="true"/>
		</view>
		<view class="sliderSet">
			<text>音调</text>
			<slider value="5" min="0" max="15" @change="sliderChange" show-value="true"/>
		</view>
		<view class="sliderSet">
			<text>音调</text>
			<slider value="5" min="0" max="9" @change="sliderChange" show-value="true"/>
		</view>
		<u-cell-group>
			<u-cell  title="音色选择" isLink>
				<u-icon slot="icon" size="30" name="volume-fill"></u-icon>
				<!-- <u-badge count="99" :absolute="false" slot="right-icon"></u-badge> -->
			</u-cell>
		</u-cell-group>
	</view>
</template>

<script>
	export default {
		data () {
			return {
				
			}
		},
		
		methods: {
			sliderChange(e) {
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
	
</style>