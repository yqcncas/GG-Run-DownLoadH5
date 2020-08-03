<template>
	<view>
		<view class="container">
			<image class="icon" src="/static/logo.png" mode="aspectFill"></image>
			<view class="title">GG来送 App</view>
			
			<view class="handle-container">
				<view @click="handleDownIos">ios下载</view>
				<view @click="handleDownAndroid">安卓下载</view>
			</view>
			<view class="tip">安卓可直接安装（请在浏览器打开）</view>
		</view>
		<tip-open-browser :openBrowserFlag="openBrowserFlag"></tip-open-browser>
	</view>
</template>

<script>
	import tipOpenBrowser from './open-browser'
	export default {
		data() {
			return {
				androidDown: 'http://yonghu.gglaisong.com/GGlaisong.apk',
				iosDown: 'https://apps.apple.com/cn/app/gg%E6%9D%A5%E9%80%81/id1494559484',
				openBrowserFlag: false // 是否微信浏览器
			};
		},
		components: {
		    tipOpenBrowser
		},
		methods:{
			// 获取安卓 ios 下载链接
			getDownPath() {
				if(/android/i.test(navigator.userAgent)){
				   //Android平台下浏览器
				    if (this.isWeiXin()) this.openBrowserFlag = true 
					else {
						 this.openBrowserFlag = false
						 window.location.href = this.androidDown
					}
				   
				}
			},
			// 点击安卓下载
			handleDownAndroid() {
				location.href = this.androidDown
			},
			// 点击ios下载
			handleDownIos() {
				location.href = this.iosDown
			},
			// 判断是否微信浏览器
			isWeiXin() {
				var ua = window.navigator.userAgent.toLowerCase()
				if (ua.match(/MicroMessenger/i) == 'micromessenger') {
					return true
				} else {
					return false
				}
			}
		},
		onLoad() {
			this.getDownPath()
		    if(/(iPhone|iPad|iPod|iOS)/i.test(navigator.userAgent)){
		       //iOS平台下浏览器
				window.location.href = "https://apps.apple.com/cn/app/gg%E6%9D%A5%E9%80%81/id1494559484"
		    }
		}
	}
</script>

<style lang="less">
	.container {
		text-align: center;
		background: #FFFFFF;
		
		.bg {
			position: absolute;
			left: 0;
			top: 0;
			width: 100vw;
			height: 100vh;
			z-index: -1;
		}
		
		.icon {
			width: 358upx;
			height: 358upx;
			margin: 0 auto;
			margin-top: 90upx;
		}
		
		.title {
			font-size: 50upx;
			color: #fff;
			text-align: center;
			margin-bottom: 50upx;
			color: #333333;
		}
		
		.content {
			font-size: 28upx;
			color: #fff;
			width: 600upx;
			margin: 0 auto;
			margin-bottom: 70upx;
		}
		
		.handle-container {
			
			view {
				background: #5468FF;
				width: 690upx;
				height: 100upx;
				border-radius: 55upx;
				display: flex;
				justify-content: center;
				align-items: center;
				color: #fff;
				font-size: 34upx;
				margin: 0 auto;
				margin-bottom: 40upx;
				
				&:active {
					background: #512C5B;
					color: #999999;
				}
			}
			
			& > view {
				margin-bottom: 30upx;
			}
		}
		
		.tip {
			font-size: 28upx;
			color: #333333;
		}
	}
</style>
