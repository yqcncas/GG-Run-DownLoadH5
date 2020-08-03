<template>
	<view class="content">
		<view class="content-box" >
			<input type="number" maxlength="11" @blur="checkPhoneNumber" v-model="userPhone" class="content-item" placeholder="请输入手机号" placeholder-style="font-family: PingFangSC-Regular;font-size: 14px;color: rgba(9,2,62,0.30);">
			<view class="code">
				<input type="number" v-model="code" class="code-item" placeholder="请输入验证码" placeholder-style="font-family: PingFangSC-Regular;font-size: 14px;color: rgba(9,2,62,0.30);">
				<view class="getCode" @tap="getCode" :class="{timer:computedTimer}">{{getYzm?'获取验证码':computedTimer+'s'}}</view>
			</view>
			<input type="password" v-model="userPwd" class="content-item" placeholder="请输入密码" placeholder-style="font-family: PingFangSC-Regular;font-size: 14px;color: rgba(9,2,62,0.30);">
			<input type="password" v-model="againUserPwd" class="content-item" placeholder="请重复密码" placeholder-style="font-family: PingFangSC-Regular;font-size: 14px;color: rgba(9,2,62,0.30);">
			<view class="loginButton" @tap="loginButton">立即注册</view>
			<view class="oldUser" @click.stop = "goToDownLoad">已注册用户入口</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				userPhone: '', 
				code: '',
				userPwd: '',
				againUserPwd: '',
				getYzm: true,
				computedTimer: '',
				checkPhone: '',
				inviteCode: '',
				cid: '',
				posFlag:false
			}
		},
		onLoad(options) {
			// alert(window.location.href)
			// this.cid = plus.push.getClientInfo().clientid
		
			let res = window.location.href
			// let res = "http://yonghu.gglaisong.com/?inviteCode=I8W3MDPA&from=singlemessage#/"
			let index = res.indexOf('=')
			let index1 = res.indexOf('&')
			let index2 = res.indexOf('#')
			let index3 = res.indexOf('?')
			if (index1 != -1) {
				console.log('进入')
				let url = res.substring(0, index1)
				let urlIndex = url.indexOf('=')
				this.inviteCode = url.substr(urlIndex + 1)
			} else if (index2 != -1 && index3 != -1) {
				console.log('进入')
				let url = res.substring(0, index2)
				let urlIndex = url.indexOf('=')
				this.inviteCode = url.substr(urlIndex + 1)
			}else {
				console.log('进入')
				if(index != -1) {
					this.inviteCode = res.substr(index + 1)
				}
			}
		
			if (res.indexOf('wx_aid') != -1) {
				this.inviteCode = ""
			}
			if (res.indexOf('comp_id') != -1) {
				this.inviteCode = ""
			}
			if (res.indexOf('gdt_vid') != -1) {
				this.inviteCode = ""
			}
			if (res.indexOf('wx_traceid') != -1) {
				this.inviteCode = ""
			}
			
			console.log(this.inviteCode + "")
		},
		methods: {
			// let res = await this.$fetch(this.$api.customerPhone,{mobile:this.userPhone},'GET','form')
			getCode () {
				if (!this.getYzm) return
				
				if (this.checkPhone || this.userPhone.trim() === "") {
					uni.showToast({
						icon: 'none',
						title: '请检查手机号输入是否正确'
					})
					return
				}
				
				this.getYzm = false
				this.computedTimer = 60
				const timer = setInterval(() => {
					if (this.computedTimer === 0) {
						clearInterval(timer)
						this.getYzm = true
					} else {
						this.computedTimer--
					}
				},1000)
				
				uni.request({
				    url: 'http://admin.gglaisong.com/notoken/user/mobile_verification_code', //仅为示例，并非真实接口地址。
				    data: {
				        mobile: this.userPhone
				    },
					method:'GET',
					header:{
						'content-type':'application/x-www-form-urlencoded'
					},
				    success: (res) => {
				        console.log(res)
				    }
				});
			},
			checkPhoneNumber() {
				let reg = /^1[3456789]\d{9}$/;
				if (reg.test(this.userPhone)) {
					this.checkPhone = false
					return true
				} else {
					this.checkPhone = true
					return false
				}
			},
			loginButton() {
				if (this.userPhone.trim() == '' || this.code.trim() == '' || this.userPwd == '' || this.againUserPwd == '') {
					uni.showToast({
						icon:'none',
						title:'请填写信息后进行注册'
					})
					return
				}
				if (this.checkPhone || this.userPhone.trim() === "") {
					uni.showToast({
						icon: 'none',
						title: '请检查手机号输入是否正确'
					})
					return
				}
				
				if (this.userPwd != this.againUserPwd) {
					uni.showToast({
						icon:'none',
						title: '两次输入的密码不一致'
					})
					return
				}
				
				
				console.log(this.inviteCode)
				uni.request({
				    url: 'http://admin.gglaisong.com/notoken/user/customer_register', //仅为示例，并非真实接口地址。
				    data: {
				        mobile: this.userPhone,
						checkCode: this.code,
						password:this.userPwd,
						inviteCode:this.inviteCode,
						cid:this.cid
				    },
					method:'POST',
					header:{
						'content-type':'application/x-www-form-urlencoded'
					},
				    success: (res) => {
				        console.log(res)
					
						if (res.data.code == 0) {
							uni.showToast({
								icon:'none',
								title:res.data.msg
							})
							setTimeout(()=>{
								uni.reLaunch({
									url:'../downLoad/downLoad'
								})
							},1000)
							
						} else {
							uni.showToast({
								icon:'none',
								title:res.data.msg
							})
						}
				    }
				});
				
			},
			goToDownLoad () {
				uni.reLaunch({
					url:'../downLoad/downLoad'
				})
			}
		}
	}
</script>

<style lang="less">
	page{
		width: 100%;
		height: 100%;
	}
	.content{
		width: 100%;
		height: 100%;
		background: url(../../static/img/5c2ce3859156d.png) no-repeat;
		background-size: 100% 100%;
		// position: relative;
		min-height: 1350rpx;
		.content-box{
			width: 100%;
			height: 100%;
			display: flex;
			flex-direction: column;
			// justify-content: center;
			align-items: center;
			position: static;
			z-index: 9999;
			.content-item{
				width: 520rpx;
				height: 90rpx;
				background: #FFFFFF;
				border: 1px solid rgba(84,104,255,0.10);
				border-radius: 2px;
				border-radius: 2px;
				padding-left: 30rpx;
				font-size: 14px;
				margin-bottom: 30rpx;
				box-sizing: border-box;
				
				&:first-child{
					margin-top: 288rpx;
					box-sizing: border-box;
				}
				&:nth-child(3){
					margin-top: 30rpx;
					box-sizing: border-box;
				}
			}
			.code{
				display: flex;
				position: relative;
				.code-item{
					width: 520rpx;
					height: 90rpx;
					background: #FFFFFF;
					border: 1px solid rgba(84,104,255,0.10);
					border-radius: 2px;
					border-radius: 2px;
					padding-left: 30rpx;
					font-size: 14px;
					// margin-bottom: 40rpx;
					box-sizing: border-box;
					
				}
				.getCode{
					font-family: PingFangSC-Regular;
					font-size: 14px;
					color: #5468FF;
					// text-align: center;
					position: absolute;
					right: 30rpx;
					top: 50%;
					transform: translateY(-50%);
					&.timer{
						color: #ccc;
					}
				}
			}
			.loginButton{
				width: 520rpx;
				height: 90rpx;
				line-height: 90rpx;
				background: #5468FF;
				border-radius: 22.5px;
				border-radius: 22.5px;
				box-sizing: border-box;
				font-family: PingFangSC-Regular;
				font-size: 14px;
				color: #FFFFFF;
				letter-spacing: -0.34px;
				text-align: center;
				
			}
			.oldUser{
				
				padding-top: 30rpx;
				font-size: 14px;
				color: #5468FF;
				box-sizing: border-box;
			}
		}
	}
</style>
