<template>
	<view class="container">
		<!-- 小程序头部兼容 -->
		<!-- #ifdef MP -->
		<view class="mp-search-box">
			<input @click="naviageToPage('/pages/discover/search')" class="ser-input" type="text" value="搜索课程" disabled />
		</view>
		<!-- #endif -->
		
		<!-- 头部轮播 -->
		<view class="carousel-section">
			<!-- 标题栏和状态栏占位符 -->
			<view class="titleNview-placing"></view>
			<!-- 背景色区域 -->
			<view class="titleNview-background" :style="{backgroundColor:titleNViewBackground}"></view>
			<swiper autoplay="true" interval="3000" duration="500" class="carousel" circular @change="swiperChange">
				<swiper-item v-for="(item, index) in carouselList" :key="index" class="carousel-item" @click="naviageToPage(item.url)">
					<image :src="item.imgUrl" />
				</swiper-item>
			</swiper>
			<!-- 自定义swiper指示器 -->
			<view class="swiper-dots">
				<text class="num">{{swiperCurrent+1}}</text>
				<text class="sign">/</text>
				<text class="num">{{swiperLength}}</text>
			</view>
		</view>
		<!-- 分类 -->
		<view class="cate-section">
			<view v-for="(item, index) in categoryButtomList" :key="index" @click="naviageToPage(item.url)" class="cate-item">
				<view>
					<image :src="item.imgUrl"></image>
					<text>{{item.title}}</text>
				</view>
			</view>
		</view>
		
		<view v-if="banner" @click="naviageToPage(banner.url)" class="ad-1">
			<image :src="banner.imgUrl" mode="scaleToFill"></image>
		</view>
		
		
		<scroll-view scroll-x class="bg-white nav solid-top " scroll-with-animation :style="isTop? 'position:fixed;z-index:9;top:0' :''"   >
			<view class="flex text-center">
				<view class="cu-item flex-sub text-lg text-bold" :class="index==TabCur?'text-green cur':''" v-for="(item,index) in tabList" :key="index" @tap="tabSelect" :data-id="index">
					{{item.title}}
				</view>
			</view>
		</scroll-view>
		
		<!-- swiper切换 swiper-item表示一页 scroll-view表示滚动视窗 -->
		<swiper style="min-height: 100vh;" :duration="400" :current="TabCur" @change="swiperTab" >
			<swiper-item v-for="(itemList, indexlist) in contentList" :key='indexlist'>
				<scroll-view style="height: 100%;" scroll-y="true" @scrolltolower="loadMore" >
					<!-- 每一个内容 -->
					<view class="cu-card case no-card" v-for="(item, index) in itemList" :key='index'>
						<view class="cu-item shadow">
							<view class="image">
								<image src="https://ossweb-img.qq.com/images/lol/web201310/skin/big10006.jpg"
								 mode="widthFix"></image>
								<view class="cu-tag bg-blue">史诗</view>
								<view class="cu-bar bg-shadeBottom"> <text class="text-cut">我已天理为凭，踏入这片荒芜，不再受凡人的枷锁遏制。我已天理为凭，踏入这片荒芜，不再受凡人的枷锁遏制。</text></view>
							</view>
							<view class="cu-list menu-avatar">
								<view class="cu-item">
									<view class="cu-avatar round lg" style="background-image:url(https://ossweb-img.qq.com/images/lol/web201310/skin/big10006.jpg);"></view>
									<view class="content flex-sub">
										<view class="text-grey">正义天使 凯尔</view>
										<view class="text-gray flex justify-between">
											<text>
												十天前	
											</text>
											<view class="text-gray flex justify-between" style="width: 50%;">
												<view>
													<text class="cuIcon-attentionfill <margin-lr></margin-lr>-xs"></text> 
													<text> 10 </text>
												</view>
												<view>
													<text class="cuIcon-appreciatefill margin-lr-xs"></text> 
													<text> 10 </text>
												</view>
												<view>
													<text class="cuIcon-messagefill margin-lr-xs"></text> 
													<text> 10 </text>
												</view>
											</view>
										</view>
									</view>
								</view>
							</view>
						</view>
					</view>
				</scroll-view>
			</swiper-item>
		</swiper>
		
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				TabCur:0,
				swiperCurrent: 0,
				swiperLength: 0,
				currentTab:0,
				isTop:false,// 导航栏到位置固定
				myScroll:0,
				tabList:[
						{
							title:'推荐',
							type:'suggest'
						},{
							title:'关注',
							type:'subscribe'
						},{
							title:'视频',
							type:'video'
						},{
							title:'图文',
							type:'pictures'
						},
				],
				contentList:[
					[
						{
							title:'推荐',
							type:'suggest'
						},{
							title:'关注',
							type:'subscribe'
						},{
							title:'视频',
							type:'video'
						},{
							title:'图文',
							type:'pictures'
						},
					],
					[
						{
							title:'推荐',
							type:'suggest'
						},{
							title:'关注',
							type:'subscribe'
						},{
							title:'视频',
							type:'video'
						},{
							title:'图文',
							type:'pictures'
						},
					],
				],
				titleNViewBackground: '',
				carouselList: [],
				categoryButtomList: [
					{
						"id":1,
						"url":"pages/course/kecheng",
						"imgUrl":"/static/category/kecheng.png",
						"title":"课程",
					},{
						"id":2,
						"url":"pages/course/kouyu",
						"imgUrl":"/static/category/kouyu.png",
						"title":"口语",
					},{
						"id":3,
						"url":"pages/course/tingli",
						"imgUrl":"/static/category/tingli.png",
						"title":"听力",
					},{
						"id":4,
						"url":"pages/course/xiezuo",
						"imgUrl":"/static/category/xiezuo.png",
						"title":"写作",
					},{
						"id":5,
						"url":"pages/course/yuedu",
						"imgUrl":"/static/category/yuedu.png",
						"title":"阅读",
					},{
						"id":6,
						"url":"pages/course/pengyou",
						"imgUrl":"/static/category/pengyou.png",
						"title":"朋友圈",
					},{
						"id":7,
						"url":"pages/course/shoucang",
						"imgUrl":"/static/category/shoucang.png",
						"title":"收藏夹",
					},{
						"id":8,
						"url":"pages/course/paihang",
						"imgUrl":"/static/category/paihang.png",
						"title":"排行",
					},{
						"id":9,
						"url":"pages/course/cihui",
						"imgUrl":"/static/category/cihui.png",
						"title":"词汇测试",
					},{
						"id":10,
						"url":"pages/course/zizhi",
						"imgUrl":"/static/category/zizhi.png",
						"title":"自制词书",
					},
					
				],
				banner: undefined,
				isVip: false
			}
		},
		methods: {
			async loadData() {
				const that = this
				uni.showLoading({
					title: '正在加载'
				})
				that.$api.request('integral', 'getIndexData', failres => {
					that.$api.msg(failres.errmsg)
					uni.hideLoading()
				}).then(res => {
					let data = res.data
					//轮播
					data.advertisement.t1.forEach(item => {
						if (!item.color) {
							item.color = 'rgb(205, 215, 218)'
						}
					})
					that.carouselList = data.advertisement.t1
					that.swiperLength = data.advertisement.t1.length
					that.titleNViewBackground = data.advertisement.t1[0].color
					
					//横幅
					if (data.advertisement.t3 && data.advertisement.t3.length > 0) {
						that.banner = data.advertisement.t3[0]
					}
					
					//标签
					// if (data.advertisement.t4) {
					// 	that.categoryButtomList = data.advertisement.t4
					// }
					uni.hideLoading()
				})
			},
			//点击进页面
			naviageToPage(page) {
				uni.navigateTo({
					url: page
				})
			},
			// 轮播切换
			tabSelect(e) {
				this.TabCur = e.currentTarget.dataset.id;
				//this.scrollLeft = (e.currentTarget.dataset.id - 1) * 60
			},
			swiperTab(e){
				console.log(e);
				this.TabCur = e.detail.current;
			},
		},
		onShow() {
			this.isVip = this.$api.isVip()
		},
		onLoad(options) {
			//#ifdef H5
			//H5进入，有可能是回调进来的
			if (options.code && options.state) {
				const that = this
				that.logining = true
				that.$api.request('user', 'thirdPartLogin', {
					loginType: 3,
					raw: options.code
				}, failres => {
					that.logining = false
					that.$api.msg(failres.errmsg)
				}).then(res => {
					//登录成功，重定向到指定目标
					that.logining = false
					that.$store.commit('login', res.data)
					uni.setStorageSync('userInfo', res.data)
					//重定向到
					//不能重定向到tabbar页面
					if (options.state === '/pages/discover/discover' || options.state === '/pages/user/user' 
					|| options.state === '/pages/word/word' ) {
						uni.switchTab({
							url: options.state
						})
					} else {
						uni.redirectTo({
							url: options.state
						})
					}
					
				})
			}
			//#endif
			this.loadData()
			
		},
		//页面渲染完成
		mounted() {
			console.log('mounted 组件挂载完毕状态===============》');
			const query = uni.createSelectorQuery().in(this);
			query.select('.nav').boundingClientRect(data => {
			console.log("得到布局位置信息" + JSON.stringify(data));
			console.log("节点离页面顶部的距离为" + data.top);
			
			}).exec();
			
			uni.getSystemInfoSync()({
				success: (res) => {
					// #ifdef H5
					this.myScroll = data.top+res.windowTop
					//#endif
					this.myScroll = data.top+45
					
				}	
			})
			console.log(this.myScroll)
		},
		onPageScroll:function(e){//下拉页面
			if(e.scrollTop > this.myScroll){
				console.log(e)
				this.isTop = 1
			}else{
				
				this.isTop = 0
			}
		},
		// #ifndef MP
		// 标题栏input搜索框点击
		onNavigationBarSearchInputClicked: async function(e) {
			uni.navigateTo({
				url: '/pages/discover/search'
			})
		},
		// #endif
	}
</script>

<style lang="scss">
	/* #ifdef MP */
	page{
		.cate-section{
			position:relative;
			z-index:5;
			border-radius:16upx 16upx 0 0;
			margin-top:-20upx;
		}
		.carousel-section{
			padding: 0;
			.titleNview-placing {
				padding-top: 0;
				height: 0;
			}
			.carousel{
				.carousel-item{
					padding: 0;
				}
			}
			.swiper-dots{
				left:45upx;
				bottom:40upx;
			}
		}
	}
	/* #endif */
	
	
	page {
		background: #f5f5f5;
	}
	.m-t{
		margin-top: 16upx;
	}
	/* 头部 轮播图 */
	.carousel-section {
		position: relative;
		padding-top: 10px;
	
		.titleNview-placing {
			height: var(--status-bar-height);
			padding-top: 44px;
			box-sizing: content-box;
		}
	
		.titleNview-background {
			position: absolute;
			top: 0;
			left: 0;
			width: 100%;
			height: 426upx;
			transition: .4s;
		}
	}
	.carousel {
		width: 100%;
		height: 350upx;
	
		.carousel-item {
			width: 100%;
			height: 100%;
			padding: 0 28upx;
			overflow: hidden;
		}
	
		image {
			width: 100%;
			height: 100%;
			border-radius: 10upx;
		}
	}
	.swiper-dots {
		display: flex;
		position: absolute;
		left: 60upx;
		bottom: 15upx;
		width: 72upx;
		height: 36upx;
		background-image: url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAMgAAABkCAYAAADDhn8LAAAAGXRFWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAAyZpVFh0WE1MOmNvbS5hZG9iZS54bXAAAAAAADw/eHBhY2tldCBiZWdpbj0i77u/IiBpZD0iVzVNME1wQ2VoaUh6cmVTek5UY3prYzlkIj8+IDx4OnhtcG1ldGEgeG1sbnM6eD0iYWRvYmU6bnM6bWV0YS8iIHg6eG1wdGs9IkFkb2JlIFhNUCBDb3JlIDUuNi1jMTMyIDc5LjE1OTI4NCwgMjAxNi8wNC8xOS0xMzoxMzo0MCAgICAgICAgIj4gPHJkZjpSREYgeG1sbnM6cmRmPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5LzAyLzIyLXJkZi1zeW50YXgtbnMjIj4gPHJkZjpEZXNjcmlwdGlvbiByZGY6YWJvdXQ9IiIgeG1sbnM6eG1wTU09Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC9tbS8iIHhtbG5zOnN0UmVmPSJodHRwOi8vbnMuYWRvYmUuY29tL3hhcC8xLjAvc1R5cGUvUmVzb3VyY2VSZWYjIiB4bWxuczp4bXA9Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC8iIHhtcE1NOkRvY3VtZW50SUQ9InhtcC5kaWQ6OTk4MzlBNjE0NjU1MTFFOUExNjRFQ0I3RTQ0NEExQjMiIHhtcE1NOkluc3RhbmNlSUQ9InhtcC5paWQ6OTk4MzlBNjA0NjU1MTFFOUExNjRFQ0I3RTQ0NEExQjMiIHhtcDpDcmVhdG9yVG9vbD0iQWRvYmUgUGhvdG9zaG9wIENDIDIwMTcgKFdpbmRvd3MpIj4gPHhtcE1NOkRlcml2ZWRGcm9tIHN0UmVmOmluc3RhbmNlSUQ9InhtcC5paWQ6Q0E3RUNERkE0NjExMTFFOTg5NzI4MTM2Rjg0OUQwOEUiIHN0UmVmOmRvY3VtZW50SUQ9InhtcC5kaWQ6Q0E3RUNERkI0NjExMTFFOTg5NzI4MTM2Rjg0OUQwOEUiLz4gPC9yZGY6RGVzY3JpcHRpb24+IDwvcmRmOlJERj4gPC94OnhtcG1ldGE+IDw/eHBhY2tldCBlbmQ9InIiPz4Gh5BPAAACTUlEQVR42uzcQW7jQAwFUdN306l1uWwNww5kqdsmm6/2MwtVCp8CosQtP9vg/2+/gY+DRAMBgqnjIp2PaCxCLLldpPARRIiFj1yBbMV+cHZh9PURRLQNhY8kgWyL/WDtwujjI8hoE8rKLqb5CDJaRMJHokC6yKgSCR9JAukmokIknCQJpLOIrJFwMsBJELFcKHwM9BFkLBMKFxNcBCHlQ+FhoocgpVwwnv0Xn30QBJGMC0QcaBVJiAMiec/dcwKuL4j1QMsVCXFAJE4s4NQA3K/8Y6DzO4g40P7UcmIBJxbEesCKWBDg8wWxHrAiFgT4fEGsB/CwIhYE+AeBAAdPLOcV8HRmWRDAiQVcO7GcV8CLM8uCAE4sQCDAlHcQ7x+ABQEEAggEEAggEEAggEAAgQACASAQQCCAQACBAAIBBAIIBBAIIBBAIABe4e9iAe/xd7EAJxYgEGDeO4j3EODp/cOCAE4sYMyJ5cwCHs4rCwI4sYBxJ5YzC84rCwKcXxArAuthQYDzC2JF0H49LAhwYUGsCFqvx5EF2T07dMaJBetx4cRyaqFtHJ8EIhK0i8OJBQxcECuCVutxJhCRoE0cZwMRyRcFefa/ffZBVPogePihhyCnbBhcfMFFEFM+DD4m+ghSlgmDkwlOgpAl4+BkkJMgZdk4+EgaSCcpVX7bmY9kgXQQU+1TgE0c+QJZUUz1b2T4SBbIKmJW+3iMj2SBVBWz+leVfCQLpIqYbp8b85EskIxyfIOfK5Sf+wiCRJEsllQ+oqEkQfBxmD8BBgA5hVjXyrBNUQAAAABJRU5ErkJggg==);
		background-size: 100% 100%;
	
		.num {
			width: 36upx;
			height: 36upx;
			border-radius: 50px;
			font-size: 24upx;
			color: #fff;
			text-align: center;
			line-height: 36upx;
		}
	
		.sign {
			position: absolute;
			top: 0;
			left: 50%;
			line-height: 36upx;
			font-size: 12upx;
			color: #fff;
			transform: translateX(-50%);
		}
	}
	/* 分类 */
	.cate-section {
		display: flex;
		justify-content: space-around;
		align-items: center;
		flex-wrap:wrap;
		padding: 30upx 22upx; 
		background: #fff;
		.cate-item {
			display: flex;
			flex-direction: column;
			align-items: center;
			font-size: $font-sm + 2upx;
			color: $font-color-dark;
		}
		/* 原图标颜色太深,不想改图了,所以加了透明度 */
		view{
			width: 120upx;
			height: 120upx;
			display: flex;
			justify-content: space-around;
			align-items: center;
			flex-wrap:wrap;
			margin-top:20upx;
			margin-bottom:10upx;
			image {
				width: 88upx;
				height: 88upx;
				margin-bottom: 14upx;
				border-radius: 50%;
				opacity: .7;
				box-shadow: 4upx 4upx 20upx rgba(250, 67, 106, 0.3);
			},
			text{
			}
		}
	}
	.ad-1{
		width: 100%;
		height: 210upx;
		padding: 10upx 0;
		background: #fff;
		image{
			width:100%;
			height: 100%; 
		}
	}

</style>
