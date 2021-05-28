<template>
	<view>
		<button type="primary" @click="setStorage">存储数据</button>
		<button type="primary" @click="getStorage">获取数据</button>
		<view 
			v-for="(item, index) in list" 
			:key="index"
			class="box-item"
			>
			{{item}}
		</view>
		<!-- <button @click="pullDown">下拉刷新</button> -->
	</view>
</template>

<script>
	export default {
		data() {
			return {
				list: ['前端', 'java', 'UI', '测试', '大数据', '前端', 'java', 'UI', '测试', '大数据']
			}
		},
		onPullDownRefresh() {
			console.log('触发了下拉刷新');
			setTimeout(()=> {
				this.list = ['UI', '测试', '前端', 'java', '大数据'];
				uni.stopPullDownRefresh();
			}, 2000);
		},
		onReachBottom() {
			console.log('页面触底了');
			this.list = [...this.list, ...['前端', 'java', 'UI', '测试', '大数据']]
		},
		methods: {
			pullDown() {
				uni.startPullDownRefresh()
			},
			setStorage() {
				uni.setStorage({
					key: 'id',
					data: 80,
					success() {
						console.log('存储成功')
					}
				})
			},
			getStorage() { 
				uni.getStorage({
					key: 'id',
					success(res) {
						console.log('获取成功', res);
					}
				})
			}
		}
	}
</script>

<style>
.box-item {
	height: 100px;
	line-height: 100px;
}
</style>
