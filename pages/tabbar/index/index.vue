<template>
	<view class="content">
		<view class="section-box">
			<button type="primary" @tap="uploadImg">上传图片</button>
			<view class="img-list">
				<view class="img-item" v-for="(item, index) in imgArr" :key ="index">
					<image  :src="item" mode="widthFix" @tap="previewImg(item)"></image>
				</view>
			</view>
		</view>
		<view class="section-box">
			<button type="primary" @tap="uploadVedio">视频上传</button>
		</view>
		
		<view class="section-box">
			<view>扫码结果: {{ scanResult }}</view>
			<button type="primary" @tap="scanCode">扫一扫</button>
		</view>
		
		<view class="section-box">
			<view>longitude: {{ longitude }}</view>
			<view>latitude: {{ latitude }}</view>
			<button type="primary" @tap="getlocation">获取地理位置</button>
		</view>
		
		<view class="section-box">
			<view>富文本编辑器</view>
			<editor placeholder="公司介绍" id="editor" class="ql-container" @ready="onEditorReady"></editor>
		</view>
		
	</view>
</template>

<script>
	// 注册一个进度条
	
	export default {
		data() {
			return {
				imgArr: [],
				src: '',
				
				vedioUrl: '',
				
				gongsijieshao: '',
				
				scanResult: '',
				longitude: '',
				latitude: '',
				
			}
		},
		onLoad() {
			
		},
		methods: {
			uploadImg() {
				uni.chooseImage({
				    count: 4, //默认9
				    sizeType: ['original', 'compressed'], // 可以指定是原图还是压缩图，默认二者都有
				    success: res => {
				        // tepFliePaths 保存图片路径 
						this.imgArr = res.tempFilePaths
				        
				    }
				});
			},
			previewImg(current) {
				uni.previewImage({
					current: current,
					urls: this.imgArr,
					loop: true,
					indicator: "number"
				})
			},
			uploadVedio() { 
				uni.chooseVideo({
					count: 1,
					sourceType: ['camera', 'album'],
					success: res => {
						this.vedioUrl = res.tempFilePath;
					}
				});
			},
				
			scanCode() {
				// 允许从相机和相册扫码
				uni.scanCode({
					success: res => {
						this.scanResult = res.result;
						console.log('条码类型：' + res.scanType);
						console.log('条码内容：' + res.result);
					}
				});
			},
			getlocation() {
				uni.getLocation({
				    type: 'wgs84',
				    success: res => {
						this.longitude = res.longitude;
						this.latitude = res.latitude;
						// console.log('res' + res);
				        // console.log('当前位置的经度：' + res.longitude);
				        // console.log('当前位置的纬度：' + res.latitude);
				    },
					fail: error => {
				        console.log('error：' + error);
				    },
				});
			},
			/* 富文本编辑器 */
			onEditorReady() {
			    uni.createSelectorQuery().select('#editor').context((res) => {
			        this.editorCtx = res.context
			    }).exec()
			},
			editorChange(e) {
				this.gongsijieshao = e.detail.html
			}
			

		}
	}
</script>

<style lang="scss">
	.content {
		padding: 30upx;
	}

	.section-box {
		margin-bottom: 30upx;
	}
	.img-list {
		display: flex;
		padding: 20upx;
		.img-item {
			width: 200upx;
			height: 200upx;
			margin-right: 30upx;
			// height: 100upx;
			image {
				width: 100%;
				height: 100%;
			}
		}
	}
	.ql-container { 
		background-color: #EBEBEB; 
		width: 100%; 
		padding: 10upx;
	}
		
</style>
