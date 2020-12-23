<template>
	<view class="content">
		<image class="logo" src="/static/logo.png"></image>
		<view class="text-area">
			<text class="title">{{title}}</text>
		</view>
	</view>
</template>

<script>
	
	import hello from "@/components/HelloWorld.vue"
	export default {
		data() {
			return {
				title: 'Hello'
			}
		},
		onLoad() {

		},
		methods: {



             getdata:function(){
							var obj=this;
							
							this.$http.get("http://192.168.0.115:6688/").then(function(data){
								obj.items=data.body.books;
								//console.log(data.body.books);
							});
              					},
								
								
			
			// 方式一
			sendRequest: function() {
				this.loading = true
				this.$api.test({noncestr: Date.now()}).then((res)=>{
					this.loading = false;
					console.log('request success', res)
					uni.showToast({
						title: '请求成功',
						icon: 'success',
						mask: true
					});
					this.res = '请求结果 : ' + JSON.stringify(res);
				}).catch((err)=>{
					this.loading = false;
					console.log('request fail', err);
				})
			},
			
			
			//接口二:奖品列表
			
			sendRequest: function() {
				this.loading = true
				this.$api.tests({noncestr: Date.now()}).then((res)=>{
					this.loading = false;
					console.log('request success', res)
					uni.showToast({
						title: '请求成功',
						icon: 'success',
						mask: true
					});
					this.res = '请求结果 : ' + JSON.stringify(res);
				}).catch((err)=>{
					this.loading = false;
					console.log('request fail', err);
				})
			},
			
			
			
			//方式二
			async sendRequest1() {
				this.loading = true
				let res = await this.$api.test({noncestr: Date.now()});
				this.loading = false;
				this.res = '请求结果 : ' + JSON.stringify(res);
			}
			
			

		}
	}
</script>

<style>
	.content {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
	}

	.logo {
		height: 200rpx;
		width: 200rpx;
		margin-top: 200rpx;
		margin-left: auto;
		margin-right: auto;
		margin-bottom: 50rpx;
	}

	.text-area {
		display: flex;
		justify-content: center;
	}

	.title {
		font-size: 36rpx;
		color: #8f8f94;
	}
</style>
