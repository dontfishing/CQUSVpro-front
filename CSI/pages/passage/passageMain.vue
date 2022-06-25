<template>
	<view class="passageGround">
		<!-- 搜索区域 -->
		<view class="searchArea">
			<!-- 搜索栏 -->
			<view class="searchBar">
				<u-search placeholder="请输入搜索关键词" borderColor="#e9e9e9" shape="square" v-model="keyword" clearabled
					bgColor="white"></u-search>
			</view>
		</view>

		<!-- 具体内容 -->
		<view class="contentArea">
			<u-list @scrolltolower="scrolltolower">
				<u-list-item v-for="(item, index) in indexList" :key="index">
					
					<u-avatar slot="icon" shape="square" size="35" :src="item.url"
						customStyle="margin: -3px 5px -3px 0"></u-avatar>
					<view></view>
					<u-cell >
						<view class="details">
						<view class="column1">
							用户名
						</view>
						<view class="column2">
						</view>
						</view>
					</u-cell>

					<u-cell title="文章标题" label="描述信息">
					</u-cell>
				</u-list-item>
			</u-list>
		</view>

	</view>
</template>

<script>
	export default {
		data() {
			return {
				keyword: '',
				indexList: [],
				urls: []
			}
		},
		onLoad() {
			this.loadmore()
		},
		methods: {
			scrolltolower() {
				this.loadmore()
			},
			loadmore() {
				for (let i = 0; i < 4; i++) {
					this.indexList.push({
						url: this.urls[uni.$u.random(0, this.urls.length - 1)]
					})
				}
			}
		},
	}
</script>

<style>
	.passageGround {}
	.searchArea {
		background-color: #e9e9e9;
	}
	.searchBar {
		margin: 5px 10px 10px 5px;
	}
</style>