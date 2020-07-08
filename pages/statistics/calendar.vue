<template>
	<view>
		<model-calendar
		:sendYear="toYear" :sendMonth="toMonth"
		:dataSource="signData" :totalNum="sumCount"
		 @dateChange="getData" @clickChange="clickRegister"	 >
		 </model-calendar>
		 
		 <view class='count'>
		 	<text>已坚持打卡</text>
		 	<view class='daynumber'>
		 		<text class='number'>{{sumCount}}</text>
		 		<text class='day'>天</text>
		 	</view>
		 	<view>本月累积打卡<text class="monthSum">{{signData.length}}</text>天</view>
		 	<text>再接再厉，继续努力!</text>
		 </view>
		 <view v-if="signFlag" class="padding flex flex-direction">
		 	<button class="cu-btn bg-gradual-green margin-tb-sm lg" @click="shareEvn">今日签到</button>
		 </view>
		 
		 <view class="share-pro" >
		 	<view class="share-pro-mask" v-if="deliveryFlag"></view>
		 	<view class="share-pro-dialog" :class="deliveryFlag?'open':'close'" >
		 		<view class="close-btn" @tap="closeShareEvn">
		 			<span class="font_family">&#xe6e6;</span>
		 		</view>
		 		<view class="share-pro-title">分享</view>
		 		<view class="share-pro-body">
		 			<view class="share-item">
		 				<button open-type="share">
		 					<view class="font_family share-icon">&#xe6fa;</view>
		 					<view >分享给好友</view>
		 				</button>
		 			</view>
		 			<view class="share-item" @tap="createCanvasImageEvn">
		 				<view class="font_family share-icon">&#xe6f9;</view>
		 				<view >生成分享图片</view>
		 			</view>
		 		</view>
		 	</view>
		 </view>
		 <hchPoster ref="hchPoster" :canvasFlag.sync="canvasFlag" @cancel="canvasCancel" :posterObj.sync="posterData"/>
		 <view :hidden="canvasFlag"><!-- 海报 要放外面放组件里面 会找不到 canvas-->
		 	<canvas class="canvas"  canvas-id="myCanvas" ></canvas><!-- 海报 -->
		 </view>
		 
	</view>
</template>

<script>
	import modelCalendar from '@/components/Calendar.vue';
	import hchPoster from '@/components/hch-poster/hch-poster.vue'
	export default {
		
		data() {
			return {
				toYear: parseInt(new Date().getFullYear()), //本日
				toMonth: parseInt(new Date().getMonth() + 1), //本月
				sumCount: 0,
				signFlag: true,
				deliveryFlag: false,
				canvasFlag:true,
				signData: [],
				posterData:{},
			}
		},
		methods: {
			//获取当前用户该任务的签到数组
			getData(val) {
				let y=val.split('-')[0];
				let m=val.split('-')[1];
				//this.$http.postHttp("Comment/GetRecord", {//可以通过后台接口去获取你的打卡数据
				// 	Year: y,
				// 	Month: m,
				// }, (res) => {
				//console.log(res);
				this.sumCount = 88; //res.SumCount		
				if (y == this.toYear && m == this.toMonth) {
					let num=["02","03","06","08","12","15"],newSign=[],today=new Date().getDate();
					
					for(let i=0;i<6;i++){
						if(parseInt(num[i])>today){
							break;
						}
						newSign.push(y+"-"+m+"-"+num[i])
					}
					this.signData = newSign;
				} else {
				 	this.signData = [];
				 }
				// })
			},
			
			// 分享弹窗
			shareEvn() {
				this.deliveryFlag = true;
			},
			// 关闭分享弹窗
			closeShareEvn() {
				this.deliveryFlag = false;
			},
			// 取消海报
			canvasCancel(val){
				this.canvasFlag=val;
			},
			createCanvasImageEvn(){
				// 这个是固定写死的小程序码
				Object.assign(this.posterData,
				{
					url:api.HOST+'/static/images/hb.jpg',//商品主图
					icon:api.HOST+'/static/images/rank-select.png',//奖杯图标
					title:"这是我的朗读成就，等你挑战哦",//标题
					discountPrice:this.reads,//金币
					orignPrice:this.scores,//积分
					code:api.HOST+'/static/images/ha.jpg',//小程序码
				})
				this.$forceUpdate();//强制渲染数据
				setTimeout(()=>{
					this.canvasFlag=false;//显示canvas海报
					this.deliveryFlag = false;//关闭分享弹窗
					this.$refs.hchPoster.createCanvasImage();//调用子组件的方法
				},500)				
			},
			
			navTo(url) {
				//TODO 使用时放开
				// if (!this.hasLogin) {
				// 	url = '/pages/public/login';
				// }
				uni.navigateTo({
					url
				})
			},
		},
		created() {
			//获取当前用户当前任务的签到状态  			
			this.getData(this.toYear+"-"+this.toMonth);
		},
		components:{
			modelCalendar,
			hchPoster
		}
	}
</script>

<style>
	.count .daynumber {
		display: flex;
		flex-direction: row;
		justify-content: center;
	}

	.count .daynumber .day {
		margin-top: 50rpx;
	}

	.count {
		margin: 20rpx;
		padding: 30rpx;
		display: flex;
		text-align: center;
		border-radius: 10px;
		flex-direction: column;
		justify-content: center;
		background-color: #fff;
		align-items: center;
		
	}

	.count .number {
		color: #fff;
		font-size: 60rpx;
		background-color: #94db98;
		width: 100rpx;
		height: 100rpx;
		border-radius: 50%;
		display: flex;
		flex-direction: column;
		justify-content: center;
		margin: 20rpx;
	}

	.monthSum {
		color: red;
		font-size: 40rpx;
	}

	.count text {
		margin: 10rpx;
	}
</style>
