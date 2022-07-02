<template>
	<view>
		<view class="picker">
			<u-button @click="show=true">选择显示内容</u-button>
			<u-picker :show="show" :columns="columns" @change="change" @close="close" @cancel="cancel"
				@confirm="confirm" :closeOnClickOverlay="true"></u-picker>
		</view>
		<view>
			<view v-show="showWhat[0]" class="ageMain">
			</view>
			<view v-show="showWhat[1]" class="sexMain">
				<view style="height: 300px;background-color: pink;"></view>
			</view>
		</view>
	</view>
</template>

<script>
	import * as echarts from 'echarts'
	echarts.use([
		LegendComponent,
		TitleComponent,
		TooltipComponent,
		GridComponent,
		DatasetComponent,
		TransformComponent,
		LineChart,
		barChart,
		LabelLayout,
		UniversalTransition,
		CanvasRenderer
	]);
	export default {
		data() {
			return {
				show: false,
				showWhat: [false, false],
				columns: [
					['年龄', '性别']
				]
			}
		},
		onLoad() {
			let Token = uni.getStorageSync('login_token');
			uni.request({
				url: 'http://106.14.62.110:8080/admin/usersInfo',
				data: {
					token: Token
				},
				method: 'POST',
				success: (res) => {
					console.log(JSON.stringify(res.data));
				}
			})
		},
		methods: {
			change(e) {},
			close() {
				console.log(1);
			},
			confirm(e) {
				this.showWhat = [false, false];
				let n = e.indexs[0];
				this.showWhat[n] = true;
			},
			cancel() {
				console.log(1);
			}
		},
		components: {
			jpCharts
		},
	}
</script>

<style>
</style>
