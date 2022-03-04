<template>
	<view>
		<z-paging ref="paging" v-model="dataList" @query="queryList" auto-show-back-to-top>
			<view slot="top" class="top">
				<view>总共25条数据，每页10条，共3页</view>
				<view>加载第2页后，调用refresh，此时显示"没有更多"，不能加载第3页了</view>
			</view>
			<view v-for="(item,index) in dataList" :key="index" class="item">
				{{item.title}}
			</view>
		</z-paging>

		<view class="refresh" @click="$refs.paging.refresh()">refresh</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				dataList: [],
			}
		},
		onLoad() {

		},
		methods: {
			async queryList(page, pagesize) {
				const title = `第${page}页，每页${pagesize}条`;
				uni.showLoading({
					title,
					mask: false
				});
				console.log(title);
				// console.log("page: ",page);
				// console.log("pagesize: ",pagesize);

				const total = 25;
				const maxPage = Math.ceil(total / pagesize);
				const dataLen = page < maxPage ? pagesize : total % pagesize;
				const list = new Array(dataLen)
					.fill(1)
					.map((_, i) => ({
						title: `第${(page-1)*pagesize + i+1}条数据`
					}));

				setTimeout(() => {
					uni.hideLoading();
					this.$refs.paging.completeByTotal(list, total);
				}, 2000);
			}
		}
	}
</script>

<style>
	.item {
		padding: 100rpx 20rpx;
		text-align: center;
		border-bottom: 1px solid #EEEEEE;
	}

	.top {
		background-color: #007AFF;
		color: #FFFFFF;
		padding: 20rpx;
		font-size: 25rpx;
	}
	.refresh {
		position: fixed;
		bottom: 50rpx;
		right: 50rpx;
		background-color: #F0AD4E;
		padding: 20rpx;
		border-radius: 20rpx;
	}
</style>
