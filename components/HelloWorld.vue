<template>
	<view class="conbox">
		<view class="container">
			<!-- 背景 -->
			<image src="../static/bj.jpg" class="cont" mode=""></image>
			<!-- <image src="../static/caidai.png" class="caidai" mode=""></image> -->

			<view class="header">
				<view class="header-title">
					<view class="leftt">
						剩余 <text style="color: ;">{{chishu}}</text>次
					</view>
					<view class="rightt" @click="getmyPrize()">
						<image src="../static/record.png" mode="aspectFit"></image>
					</view>
				</view>
			</view>
			<view class="main">
				<view class="canvas-container" id="zhuanpano">
					<!-- 圆盘内容 canvas-container-->
					<view :animation="animationData" class="canvas-content" style="">
						<view class="canvas-list">
							<view class="canvas-item" :style="[{zIndex:index2}]" v-for="(iteml,index2) in awardsList" :key="index2">
								<view class="canvas-item-text" :style="[{transform:'rotate('+iteml.turn+')'}]">
									<text :class="index2%2==0?'aa':''">{{iteml.award}}</text>
									<!-- <img src="[{iteml.img}]" mode="aspectFit"> -->
									<image :src="iteml.img" :class="index2<2? 'aaa':'canvas-item-text-imgg'"></image>

								</view>
							</view>
						</view>
					</view>

				</view>
				<view class="a1">
					<view class="canvas-arrow"> </view>
					<view class="canvas-btn" v-bind:class="btnDisabled">
						<image class="mediumpan" src="../static/mediumpan.png" @click="play()"></image>
						<image class="arrow2x" src="../static/arrow2x.png"></image>

					</view>
				</view>
			</view>
			<!-- 现金抽奖 -->
			<view class="typecheckbox">

			</view>


			<!-- 返回按钮，消息框 -->
			<view class="navigation">
				<view class="returnkey">
					<image src="" mode=""></image>
				</view>
				<view class="page-section-spacing hurdle">

					<swiper class="swiper" :indicator-dots="indicatorDots" :autoplay="autoplay" :interval="interval" :duration="duration"
					 :vertical="true" :circular="true">
						<swiper-item v-for="(data,index) in lunbolist" :key="index">
							<view class="swiper-item hurdle-in">
								<image :src="data.avatar" mode=""></image>
								<text class="information">用户<text>{{data.userName}}</text>抽中了</text>
								<text class="information2">{{data.prize}}</text>
							</view>

						</swiper-item>


					</swiper>

					<!-- <image src="" mode=""></image>
						<text class="information">用户**抽中了</text>
						<text class="information2">1000每聊币</text>
					</view> -->

				</view>
			</view>
			<!-- 弹框1 抽中-->



			<view class="app-modal" :style="[{display:dis}]" @touchmove.stop.prevent>
				<view class="app-modal__container">

					<view class="twindow">
						<image src="../static/twindow.png"></image>
					</view>

					<view class="app-modal__container__header">
						<view :class="awardIndex<2?'picture4':'picture1'">
							<image :src="contentlist.img"></image><!-- 中奖图片调用contentlist.img -->
						</view>
						<view class="picture2">
							<image src="../static/share.png" mode=""></image>
						</view>
						<view class="app-modal__container__footer-left" hover-class="app-modal__container__footer-hover"
						 :hover-start-time="20" :hover-stay-time="70">
							<image class="" src="../static/cancel.png" @click="concel"></image>
						</view>

						<view class="app-modal__container__content">

							<view class="app-modal__container__content__text">
								<text class="textone">抽中</text>
								<text class="texttwo">{{contentlist.name}}</text>

							</view>

						</view>
					</view>

					<view class="app-modal__container__footer">


					</view>
				</view>

			</view>

			<!-- 弹框一抽中结束 -->


			<!-- 弹窗二未抽中开始 -->

			<view class="nowin" :style="[{display:dis2}]" @touchmove.stop.prevent>
				<view class="nowin__container">

					<view class="nowinwindow">
						<image src="../static/nowin.png"></image>
					</view>

					<view class="nowin__container__header">

						<view class="picture3">
							<image src="../static/again.png" @click="concel4"></image>
						</view>
						<view class="nowin__container__footer-left">
							<image class="" src="../static/cancel.png" @click="concel2"></image>
						</view>
					</view>
					<view class="nowin__container__content">
						<view class="content_input">

						</view>
						<text class="nowin__container__content__text"></text>

					</view>
					<view class="nowin__container__footer">


					</view>
				</view>

			</view>

			<!-- 弹窗二未抽中结束 -->


			<!-- 奖品记录弹窗 开始-->

			<view class="zhongjiangjilu" :style="[{display:dis3}]" @touchmove.stop.prevent>
				<view class="zhongjiangjilu__container">

					<!-- 内容 -->


					<scroll-view scroll-y class="listbox">
						<text class="display" v-if="ensconce">你还没有中奖哦~</text>

						<view class="fooditem" v-for="(data,index4) in myPrizelist" :key="index4">
							<view :class="data.prizeId==6||data.prizeId==5?'rightbox-img-img':'rightbox-img'">
								<image :src="data.remarks"></image>
							</view>
							<view class="rightbox-txt">
								<view class="line1">真幸运，抽中了<text>{{data.prize}}</text></view>
								<view class="line2">{{data.createTime}}</view>


							</view>
						</view>




					</scroll-view>
					<!-- 内容 部分结束-->

					<view class="winrecord">
						<image src="../static/winrecord.png"></image>
					</view>

					<view class="cc">
						<image src="../static/cancel.png" @click="concel3"></image>
					</view>



				</view>

			</view>

			<!-- 奖品记录弹窗 结束-->

			<!-- 登录时间统计开始 -->
			<text class="count">剩余{{remain}}天</text>
			<!-- 记录登录天数 开始-->
			<view class="stylebox" :style="{'width':((loginday-1)*118+16)+'rpx'}" v-if="carry">
				<view class="cr" id="cr"></view>
				<view class="re"></view>
				<view class="cr"></view>
				<view class="re"></view>
				<view class="cr"></view>
				<view class="re"></view>
				<view class="cr"></view>
				<view class="re" id="re"></view>
				<view class="cr" id="cr2"></view>
			</view>
			<view class="stylebox2">
				<view :class="loginday>0?'daytextcolor':'daytext'">1天</view>
				<view :class="loginday>1?'daytextcolor':'daytext'">2天</view>
				<view :class="loginday>2?'daytextcolor':'daytext'">3天</view>
				<view :class="loginday>3?'daytextcolor':'daytext'">4天</view>
				<view :class="loginday>4?'daytextcolor':'daytext'">5天</view>
			</view>
			<!-- 记录登录天数 开始-->


			<view class="zanbigbox">
				<text class="zanbigbox-in">10人</text>
				<view class="zansmallbox" :style="{'width':(dianzanunmber)*62+'rpx'}">
				</view>
				<text class="zansmallbox-in">今日已赞{{dianzanunmber}}人</text>
			</view>
			<view class="pingbigbox">
				<text class="pingbigbox-in">5人</text>
				<view class="pingsmallbox" :style="{'width':(pinglununmber)*124+'rpx'}">
				</view>
				<text class="pingsmallbox-in">今日已评论{{pinglununmber}}人</text>
			</view>


			<!-- 登录时间统计结束 -->
			<view class="guize">
				<p class="guizep">1. 用户满足每天10个点赞，评论5人，连续登录5天可抽奖三次；</p>
				<p>2. 连续登录了5天，但未满足5天连续点赞评论则不能抽奖；</p>
				<p>3. 中途不能断登录、但能断点赞评论，一但断了登录则重新计算；</p>
				<p>4. 定期更新奖品，更有现金红包定期发放；</p>
				<p>5. 活动承诺100%中奖，真实有效，抽奖金币可提现；</p>
				<p>6. 刷数据行为一经发现则封号处理，请遵守抽奖规则；</p>
				<p>7. 如系统出错请联系平台解决；</p>
				<p>8. 最终解释权归平台所有。</p>
			</view>



		</view>

		<view class="wall" v-if="showwall"></view>

	</view>

</template>

<script>
	export default {
		data() {
			return {
				awardsConfig: {
                   
					isPlay: false, //是否正在抽奖
					lists: [], //奖品列表 
				},
				awardsList: {},
				animationData: {},
				btnDisabled: '',
				thanksarr: [], //存储谢谢参与的索引
				chishu: 0,
				surprisednumber: 5, //是否中奖数字
				mold: 1,
				r_flg: 0,
				myPrizelist: [], //中奖记录列表
				lunbolist: [],
				admirenumberlist: [], //点赞接口返回数据
				dis: 'none',
				dis2: 'none',
				dis3: 'none',
				pageNum: 1,
				contentlist: {},
				dianzanunmber: 0,
				pinglununmber: 0,
				loginday: 0,
				loginDays: 0,
				login: 5, //被减数
				remain: 0,
				ensconce: true, //未中奖消息是否隐藏
				showwall: false,
				carry: false,
				awardIndex:0,
				



				indicatorDots: false,
				autoplay: true,
				interval: 2500,
				duration: 500,
				vertical: true,
				userId: ''

			}
		},

		// onPageScroll(e){
		// this.howtop = e.scrollTop	
		// console.log(this.howtop);
		// },


		onLoad: function(o) {
			// 获取奖品列表

			this.userId = o.userId
			uni.setStorageSync("token", o.token)

			this.initdata()
			this.admire()
			this.getlunbo()

			//this.handleRequest()

		},
		onReady: function(e) {

		},







		methods: {





			//轮播
			getlunbo: function() {
				var that = this
				var pageNum = this.pageNum
				this.$api.lunbo({
					pageNum: pageNum,
					activityType: 1


				}).then((res) => {
					this.loading = false;

					that.lunbolist = res.data.data.records;
					console.log('request fail', that.lunbolist);
				}).catch((err) => {
					this.loading = false;
					console.log('request fail', err);
				})
			},




			//中奖记录
			getmyPrize: function() {
				var that = this
				if(that.isPlay){
					return
				}
				this.dis3 = 'block'
				that.showwall = true
				var that = this

				this.$api.test({

					activityType: 1,
					userId: that.userId

				}).then((res) => {
					this.loading = false;
                    
					that.myPrizelist = res.data.data.records;

					//var m = myPrizelist.length();
					if (that.myPrizelist.length != 0) {
						that.ensconce = false
					}
					// console.log('that.myPrizelist', that.ensconce);
					//console.log('that.myPrizelist', that.myPrizelist);
				}).catch((err) => {
					this.loading = false;
					console.log('request fail', err);
				})
				

			},
			// 查看奖品


			//},
			concel3() {
				this.dis3 = 'none'
				return
			},
			openshadow() {
				this.r_flg = 1
			},
			closeshadow() {
				this.r_flg = 0
			},

			// 初始化抽奖数据
			initdata: function() {
				var that = this



				this.$api.tests({

					activityId: 1
				}).then((res) => {
					this.loading = false;
					that.awardsConfig.lists = res.data.data.records;

					that.drawAwardRoundel();

					//console.log('request fail', that.lists );
				}).catch((err) => {
					this.loading = false;
					console.log('request fail', err);

					//console.log(err.data.data)
				})




			},
			//画抽奖圆盘  
			drawAwardRoundel: function() {
				// 拿到奖品列表
				var awards = this.awardsConfig.lists;
				var awardsList = [];
				// 每份奖品所占角度
				var turnNum = 1 / awards.length * 360; // 文字旋转 turn 值（奖盘上的固定位置）  
				// 奖项列表  
				for (var i = 0; i < awards.length; i++) {
					awardsList.push({
						turn: i * turnNum + 'deg', //每个奖品块旋转的角度
						// lineTurn: i * turnNum + turnNum / 2 + 'deg', //奖品分割线的旋转角度
						award: awards[i].name, //奖品的名字,
						type: i,
						id: awards[i].id,
						img: awards[i].img
					});
				}
				this.btnDisabled = this.chishu ? '' : 'disabled';
				this.awardsList = awardsList;
				return
			},
			//发起抽奖


			play: function() {
				var that = this;

				if (that.chishu == 0 && that.loginday == 5) {
					uni.showToast({
						title: '抽奖次数已用完',
						icon: 'none'
					});
					return

				}
				if (that.chishu == 0) {
					uni.showToast({
						title: '您还未满足抽奖条件',
						icon: 'none'
					});
					return
				}


				//接口
				this.$api.surprised({
					userId: that.userId
				}).then((res) => {
					this.loading = false;
                    
					that.surprisednumber = res.data.data;
					that.playReward();
					

					console.log('request fail', res.data.data);
				}).catch((err) => {
					this.loading = false;
					console.log('request fail', err);
				})

			},
			playReward: function() {
				var that = this

				if (that.isPlay) {
					return
				}
				that.isPlay = true
				

				// that.runDeg = 60*Math.random()
				// if (this.chishu == 0) {
				// 	uni.showToast({
				// 		title: '抽奖次数已用完',
				// 		icon: 'none'
				// 	})
				// 	return
				// }
				var awardIndex = 0 //中奖位置索引
				console.log(that.surprisednumber)
				that.awardsList.forEach(function(element, index) {
					if (element.id == that.surprisednumber) {
						awardIndex = index
						that.contentlist = that.awardsConfig.lists[awardIndex]
						that.awardIndex = awardIndex
					}
				})
				//中奖index  
				var awardsNum = that.awardsConfig.lists;
				// var awardIndex = 1 || Math.round(Math.random() * (awardsNum.length - 1)); //随机数  
				var runNum = 8; //旋转圈数 
				var duration = 13000; //时长  

				// 旋转角度  
				that.runDeg = that.runDeg || 0;
				let preDeg = that.runDeg
				that.runDeg = that.runDeg + (360 - that.runDeg % 360) + (360 * runNum - awardIndex * (360 / awardsNum.length)) +
					335
				//创建动画  
				if (process.env.VUE_APP_PLATFORM == 'h5') {
					// document.styleSheets[0]
					document.getElementById('zhuanpano').style = 'animation:rotate_before 12s 0ms ease forwards;'
					if (document.styleSheets[0].cssRules.length > 0) {
						Array.from(document.styleSheets[0].cssRules).forEach(function(element, index) {
							if (element.name == 'rotate_before') {
								// 删除上次插入的动画
								document.styleSheets[0].deleteRule(index)
							}
						})
					}


					// 插入定义的动画
					document.styleSheets[0].insertRule("@keyframes rotate_before{from{ transform: rotate(" + preDeg +
						"deg); }to{ transform: rotate(" + that.runDeg + "deg);}}", 8);

				} else {
					var animationRun = uni.createAnimation({
						duration: duration,
						timingFunction: 'ease'
					})
					animationRun.rotate(that.runDeg).step();
					that.animationData = animationRun.export();
				}
				that.btnDisabled = 'disabled';

				// 中奖提示  ==========
				var awardsConfig = that.awardsConfig;
				//var awardType =awardIndex;
				that.chishu = that.chishu - 1;
				if (awardIndex != 6) {

					setTimeout(function() {
						that.dis = 'block'
						that.showwall = true
						// that.dis = true
						that.isPlay = false;
						setTimeout(function() {
							document.getElementById('zhuanpano').style = ''
						}, 1000)

						that.btnDisabled = '';
					}.bind(that), duration);



				} else {
					setTimeout(function() {
						that.dis2 = 'block'
						that.showwall = true
						// that.dis2 = true
						that.isPlay = false;
						
						setTimeout(function() {
							document.getElementById('zhuanpano').style = ''
						}, 1000)

						that.btnDisabled = '';
					}.bind(that), duration);
				}




			},

			

			// 发起抽奖完成
			// 关闭弹窗事件
			concel() {
				this.dis = 'none'
				this.showwall = false
				
				return
			},
			concel2() {
				this.dis2 = 'none'
				this.showwall = false
				
				return
			},
			concel3() {
				this.dis3 = 'none'
				this.showwall = false

				return
			},
			concel4() {
				this.dis2 = 'none'
				this.showwall = false

				return
			},
			concel5() {

			},

			// 阻止默認事件
			preventDefault(e) {
				e.preventDefault()
			},





			//点赞接口
			admire: function() {
				var that = this


				this.$api.praise({
					userId: that.userId

				}).then((res) => {
					this.loading = false;
					that.dianzanunmber = res.data.data.click;
					if (that.dianzanunmber > 10) {
						that.dianzanunmber = 10
					}
					that.pinglununmber = res.data.data.comment;
					if (that.pinglununmber > 5) {
						that.pinglununmber = 5
					}
					that.loginDays = res.data.data.loginDays;
					that.loginday = res.data.data.days;
					if (that.loginday > 0) {
						that.carry = true
					}
					that.remain = (that.login) - (that.loginDays);
					if (that.remain < 0) {
						that.remain = 0
					}
					that.chishu = res.data.data.christmasNumber;
					if (res.data.data.christmasNumber == null) {
						that.chishu = 0
					}
					if (res.data.data.christmasNumber < 0) {
						that.chishu = 0
					}
					console.log('request fail', that.chishu);
					that.admirenumberlist = res.data.data;

					console.log('request fail', res.data.data);
				}).catch((err) => {
					this.loading = false;
					console.log('request fail', err);
				})

			},
			//点赞接口结束

		},
		watch: {
			// dis3: function (newVal)　{

			// 	if (newVal === 'block') {
			// 		document.body.style.position = 'fixed'
			// 	} else {
			// 		document.body.style.position = 'unset'
			// 	}
			// },

			// dis2: function(newVal) {

			// 	if (newVal === 'block') {
			// 		document.body.style.position = 'fixed'
			// 	} else {
			// 		document.body.style.position = 'unset'
			// 	}
			// },

			// dis: function(newVal) {

			// 	if (newVal === 'block') {
			// 		document.body.style.position = 'fixed'
			// 	} else {
			// 		document.body.style.position = 'unset'
			// 	}
			// }

		},



	}
</script>
<style scoped>
	.returnkey {
		position: absolute;
		top: 98upx;
		left: 30upx;
		z-index: auto;
	}

	.returnkey image {
		height: 68upx;
		width: 68upx;
	}

	.hurdle {
		height: 60upx;
		width: 550upx;
		position: absolute;
		top: 90upx;
		left: 100upx;
		z-index: auto;
		background-color: #333333;
		background-color: rgba(0, 0, 0, 0.5);
		text-align: center;
		border-radius: 30rpx;
	}


	.hurdle-in {

		/* line-height: 65rpx; */
		margin: 5rpx auto;
		display: block;
	}

	.hurdle-in image {
		position: relative;
		height: 36rpx;
		width: 36rpx;
		border-radius: 50%;
		top: 7rpx;
	}


	.information {

		height: 36upx;
		padding-left: 18rpx;
		font-size: 25rpx;
		color: white;
		margin-top: 13rpx;
	}

	.information text {
		color: #FFE57F;
	}

	.information2 {
		height: 36upx;
		font-size: 25rpx;
		color: #FFE57F;
		padding-left: 5rpx;
	}

	.conbox {
		width: 750rpx;
		height: 2514rpx;
		overflow-x: hidden;
		overflow-y: auto;
	}

	.container,
	image.cont {
		width: 750rpx;
		min-height: 2514rpx;
		height: auto;
		position: relative;
	}

	image.cont {
		height: 100%;
		position: absolute;
		z-index: 0;
	}

	image.caidai {
		position: absolute;
		top: 0;
		left: 0;
		width: 750upx;
		height: 1024upx;
	}

	.header {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		/* height: 246upx; */
		padding-top: 48upx;
		padding-bottom: 40upx;
		box-sizing: border-box;
		position: relative;
		z-index: auto;
	}

	.header-title {
		width: 100%;
		height: 60upx;
		display: flex;
		align-items: center;
		padding: 0 48upx;
		box-sizing: border-box;
		justify-content: space-between;
	}

	.leftt {
		position: absolute;
		top: 1215rpx;
		left: 309rpx;
		color: white;
		font-size: 36upx;
		border-radius: 26upx;

	}

	.rightt {
		position: absolute;
		top: 1269rpx;
		left: 295rpx;
		padding: 8upx 16rpx;
		z-index: auto;
	}

	.rightt image {
		height: 76rpx;
		width: 130rpx;
	}

	/* 转盘 */
	.canvas-container {
		margin-top: 392rpx;
		margin-left: 106rpx;
		position: relative;

		width: 543rpx;
		height: 543rpx;
		background: url(../static/jiangpan2x.png) no-repeat;
		background-size: cover;
		border-radius: 50%;
		-webkit-transform: rotate(0deg);
		/* 旋转角度 */
	}

	.canvas {
		width: 100%;
		height: 100%;
		display: block !important;
		border-radius: 50%;
	}

	.canvas-content {
		position: absolute;
		left: 0;
		top: 0;
		z-index: 1;
		display: block;
		width: 600upx;
		height: 600upx;
		border-radius: inherit;
	}

	.canvas-element {
		position: relative;
		z-index: 1;
		width: inherit;
		height: inherit;
		border-radius: 50%;
	}

	/* 奖盘内容 */
	.canvas-list {
		position: absolute;
		left: -23upx;
		top: -32upx;
		width: inherit;
		height: inherit;
		z-index: 9999;
		-webkit-transform: rotate(25deg);
		/* 旋转角度 */
	}

	.canvas-item {
		position: absolute;
		left: 0;
		top: 0;
		width: 100%;
		height: 100%;
		color: #e4370e;
	}

	.canvas-item-text {
		position: relative;
		display: block;
		padding-top: 40upx;
		margin: 25upx auto;
		text-align: center;
		-webkit-transform-origin: 50% 300upx;
		transform-origin: 50% 285upx;
		display: flex;
		flex-direction: column;
		align-items: center;
		color: #AD7317;
	}

	.aa {
		color: #FFFFFF;
	}

	.canvas-item-text text {

		font-size: 29rpx;
	}

	.canvas-item-text-imgg {
		width: 70rpx;
		height: 70rpx;
		padding-top: 10rpx;
	}

	.aaa {
		width: 85rpx;
		height: 100rpx;
		padding-top: 2rpx;
	}


	.canvas-litem {
		position: absolute;
		left: 300upx;
		top: 0;
		width: 3upx;
		height: 300upx;
		background-color: rgba(228, 55, 14, 0.4);
		overflow: hidden;
		-webkit-transform-origin: 50% 300upx;
		transform-origin: 50% 300upx;
	}


	/* 抽奖按钮箭头 */

	.arrow2x {
		position: absolute;
		left: 173upx;
		top: 205rpx;
		z-index: auto;
		width: 208upx;
		height: 208upx;
		border-radius: 50%;

	}

	/**  
* 抽奖按钮  
*/
	.mediumpan {
		position: absolute;
		left: 194rpx;
		top: 233rpx;
		z-index: 1;
		width: 165rpx;
		height: 165rpx;
		border-radius: 50%;


		/* background: url(../static/mediumpan.png) no-repeat; */


	}

	.canvas-btn::after {
		position: absolute;
		display: block;
		content: ' ';
		left: 50upx;
		top: -50upx;
		width: 0;
		height: 0;
		overflow: hidden;
		z-index: 400;

	}

	.canvas-btn.disabled::after {
		border-bottom-color: #b07a7b;
	}

	.canvas-btn-table {
		color: #A83FDB;
		width: 120upx;
		text-align: center;
		position: absolute;
		left: 240upx;
		top: 360upx;
		font-size: 26upx;
		background-color: #FFFFFF;
		opacity: 0.9;
	}

	/* 弹窗内容 */
	.typecheckbox {
		width: 100%;
		position: relative;
		z-index: 3;
		display: flex;
		justify-content: space-between;
		padding: 20upx;
		box-sizing: border-box;
		color: #fff;
		font-size: 28upx;
		top: -120upx;
		flex-direction: column;
		height: 180upx;
		align-items: flex-end;
	}

	.typecheckbox view {
		border: 1px solid #FF3637;
		background: transparent;
		color: #FF3637;
		display: flex;
		height: 60upx;
		width: 140upx;
		border-radius: 50upx;
		align-items: center;
		justify-content: center;
		display: flex;
		margin-left: 20upx;
	}

	.typecheckbox view.active {
		background: #FF3637;
		color: #fff;
	}

	.guize {
		width: 684rpx;
		height: 576upx;
		display: flex;
		flex-direction: column;
		position: relative;
		z-index: 3;
		font-size: 26rpx;
		background-color: #49089E;
		border-radius: 10px;
		top: 147rpx;
		left: 34rpx;
		/* box-sizing: border-box; */
		color: #e2ccff;
		line-height: 49rpx;
	}

	.guize p {
		margin-left: 26rpx;
		width: 630rpx;

	}

	.guizep {
		margin-top: 34rpx;
	}



	.shadowbox {
		width: 750upx;
		height: 100vh;
		position: fixed;
		top: 0;
		left: 0;
		z-index: 999;
		background: rgba(0, 0, 0, .6);
		display: flex;
		justify-content: center;
		align-items: center;

	}

	/* 我的奖品内容 */
	.myrewards {
		width: 600upx;
		min-height: 80upx;
		background: #FFEEDF;
		border: 10upx solid #F2692F;
		color: #333;
		font-size: 24upx;
		font-family: PingFang-SC-Medium;
		border-radius: 40upx;
		padding-bottom: 20upx;
	}

	.myrewards .title {
		font-family: PingFang-SC-Bold;
		font-size: 16px;
		color: #E4431A;
		letter-spacing: 0.57px;
		display: flex;
		padding-top: 36upx;
		justify-content: center;
	}

	.myrewards .itembox {

		max-height: 320upx;
		overflow-y: auto;
	}

	.myrewards .item {
		width: 100%;
		height: 66upx;
		padding: 0 32upx;
		box-sizing: border-box;
		display: flex;
		align-items: center;
		justify-content: space-between;
	}

	.a1 {
		position: absolute;
		left: 100upx;
		top: 500upx;

	}



	/* 弹框1样式 */

	.app-modal {
		position: absolute;
		position: fixed;
		height: 100%;
		width: 100%;
		top: 0rpx;
		z-index: 5000;
	}

	.app-modal__container {
		height: 1198rpx;
		width: 100%;

		/* background: url(../static/twindow.png) no-repeat; */
	}

	.twindow image {
		position: relative;
		top: 245rpx;
		height: 865rpx;
		width: 100%;
		z-index: 401;
	}

	.picture1 image {
		height: 98rpx;
		width: 97rpx;
		position: absolute;
		top: 631upx;
		left: 323upx;
		z-index: 401;
	}
	.picture4 image{
		width: 85rpx;
		height: 100rpx;
		position: absolute;
		top: 631upx;
		left: 332upx;
		z-index: 401;
	}

	.picture2 image {
		height: 89upx;
		width: 397upx;
		position: absolute;
		top: 888upx;
		left: 178upx;
		z-index: 401;
	}


	.app-modal__container__content__text {

		height: 50upx;
		font-size: 32rpx;
		text-align: center;
		line-height: 50upx;

		position: absolute;
		top: 810upx;
		left: 264upx;
		z-index: 500;
	}

	.app-modal__container__footer-left {
		height: 70upx;
		width: 70upx;
		/* background-color: #333333; */
		position: absolute;
		top: 1069rpx;
		left: 335rpx;

	}

	.app-modal__container__footer-left image {

		z-index: 666666;
		height: 70upx;
		width: 70upx;
	}

	.textone {
		color: #FFFFE5EE;
	}

	.texttwo {
		color: #F4E78E;
		padding-left: 15rpx;
	}

	/* 结束 */

	/* 弹框样式二开始 */

	.nowin {

		position: absolute;
		position: fixed;
		height: 100%;
		width: 100%;
		top: 0upx;
		z-index: 5000;

	}

	.nowin__container {
		height: 865upx;
		width: 100%;
		/* background: url(../static/nowin.png) no-repeat; */
	}

	.nowinwindow image {
		position: relative;
		top: 245rpx;
		height: 865upx;
		width: 100%;
		z-index: 401;
	}

	.picture3 image {
		height: 89upx;
		width: 397upx;
		position: absolute;
		top: 888upx;
		left: 178upx;
		z-index: 401;
	}



	.nowin__container__footer-left {
		height: 70upx;
		width: 70upx;
		/* background-color: #333333; */
		position: absolute;
		top: 1069upx;
		left: 335upx;

	}

	.nowin__container__footer-left image {

		z-index: 666666;
		height: 70upx;
		width: 70upx;
	}

	/* 弹框二样式结束 */


	/* 奖品记录部分样式开始 */

	.zhongjiangjilu {
		border: solid 1rpx #000000;

		display: none;
		position: absolute;
		position: fixed;
		height: 100%;
		width: 100%;
		top: 0upx;
		z-index: 5000;
	}

	.zhongjiangjilu2 {
		display: none;
		position: absolute;
		position: fixed;
		height: 2000upx;
		width: 100%;
		top: 200upx;
		z-index: 5000;
	}


	.winrecord image {
		height: 1100upx;
		width: 100%;
		z-index: 401;
		margin-top: 136rpx;
		/* box-shadow: 10px 10px 5px #888888; */
	}


	.cc image {
		height: 70upx;
		width: 70upx;
		/* background-color: #333333; */
		position: absolute;
		top: 1128upx;
		left: 350upx;
		z-index: 666666;

	}

	/* 奖品记录部分样式结束 */
	/* 内容样式 */
	.display {
		float: left;
		position: absolute;
		color: #666666;
		font-size: 26rpx;
		left: 130rpx;
	}


	.listbox {

		width: 500upx;
		height: 600upx;
		
		overflow:scroll;
		position: absolute;
		top: 446upx;
		left: 132upx;
		z-index: 44444
	}

	.fooditem {

		overflow: hidden;
		height: 76upx;
		/* width: 76rpx; */
		position: relative;
		top: -47rpx;
		padding-top: 47rpx;
	}

	.rightbox-img {
		float: left;
		height: 76rpx;
		width: 76rpx;
		background-color: #FAEED7;
	}
	.rightbox-img-img{
		float: left;
		height: 76rpx;
		width: 76rpx;
		background-color: #FAEED7;
	}

	.rightbox-img image {

		width: 54rpx;
		height: 54rpx;
		margin-top: 11rpx;
		margin-left: 11rpx;

	}
	.rightbox-img-img image{
		width: 44rpx;
		height: 54rpx;
		margin-top: 11rpx;
		margin-left: 17rpx;
	}

	.rightbox-txt {
		position: absolute;
		top: auto;
		left: 106rpx;
		float: left;
		width: 376rpx;
		height: 76rpx;



	}

	.ri {
		float: left;
		border: solid 1upx #000000;
		width: 500rpx;
		height: 76upx;
		position: absolute;
		left: 106upx;
		top: 0upx;
	}

	.line1 {
		color: #666666;
		font-size: 28rpx;
		font-weight: bold;


	}

	.line1 text {
		color: #FF4E74;
	}

	.line2 {
		color: #999999;
		padding: 5upx 0;
		font-size: 20upx;
	}

	/* 登录统计样式 */
	.zanbigbox {
		height: 44rpx;
		width: 620rpx;
		background-color: #49089E;
		margin: 0 auto;
		border-radius: 30rpx;
		position: relative;
		margin-top: 392rpx;
	}

	.zansmallbox {
		position: absolute;
		float: left;
		height: 44upx;
		width: 420upx;
		background-color: #8629FF;
		border-radius: 30px;
	}

	.zansmallbox-in {
		position: absolute;
		float: left;
		color: #FFFFFF;
		font-size: 25upx;
		line-height: 50upx;
		margin-left: 20px;

	}

	.zanbigbox-in {
		position: absolute;
		right: 20px;
		color: #FFFFFF;
		float: left;
		font-size: 25upx;
		line-height: 50upx;
	}

	.pingbigbox {
		height: 44rpx;
		width: 620rpx;
		background-color: #49089E;
		margin: 0 auto;
		border-radius: 30px;
		position: relative;
		margin-top: 20upx;
	}

	.pingsmallbox {
		position: absolute;
		float: left;
		height: 44rpx;
		width: 422rpx;
		background-color: #8629FF;
		border-radius: 30px;
	}

	.pingbigbox-in {
		position: absolute;
		right: 20px;
		color: #FFFFFF;
		float: left;
		font-size: 25upx;
		line-height: 50upx;
	}

	.pingsmallbox-in {
		position: absolute;
		color: #FFFFFF;
		font-size: 25upx;
		line-height: 50upx;
		margin-left: 20px;
	}

	.count {
		position: relative;
		float: left;
		top: 182upx;
		left: 565upx;
		color: #FFFFFF;
		font-size: 25upx;
		line-height: 50upx;
	}

	/* 记录登录天数 开始 */
	.stylebox {
		z-index: auto;
		overflow: hidden;
		position: absolute;
		top: 1570rpx;
		left: 198rpx;

		/* border: solid 2rpx gold; */
		width: 488rpx;
		/* 16 rpx   134rpx   252rpx  370rpx  488rpx   118*/
		height: 16rpx;
		/* background-color: #FF49089E; */
	}

	.cr {
		width: 16rpx;
		height: 16rpx;
		border-radius: 50%;
		background-color: #FFFFFFFF;
		float: left;
		margin-left: -8rpx;
		/*margin-left: 60px;*/
	}

	.re {
		overflow: hidden;
		float: left;
		width: 116rpx;
		height: 4rpx;
		background-color: #FFFFFFFF;
		/*border-radius: 30%;*/
		margin-top: 6rpx;
		margin-left: -7rpx;

	}

	#cr {
		margin-left: 0rpx;
	}

	#re {
		width: 110rpx;
	}

	.stylebox2 {
		overflow: hidden;
		position: absolute;
		top: 1602rpx;
		left: 121rpx;
		width: 100%;
		height: 28rpx;
	}

	.daytext {

		opacity: 0.2;
		/* 不透明度 */
		height: 25rpx;
		width: 42rpx;
		float: left;
		font-size: 18rpx;
		color: white;
		margin-left: 73rpx;
	}

	.daytextcolor {
		opacity: 1;
		height: 25rpx;
		width: 42rpx;
		float: left;
		font-size: 18rpx;
		color: white;
		margin-left: 73rpx;
	}

	/* 记录登录天数 结束 */
	.swiper {
		height: 60rpx;

	}

	.wall {
		border: solid 5rpx #000000;
		z-index: 3;
		position: absolute;
		overflow: hidden;
		width: 750rpx;
		height: 2514rpx;
		background-color: rgba(0, 0, 0, 0.5);
	}

	.container {

		overflow: hidden;
		position: absolute;

	}
</style>
