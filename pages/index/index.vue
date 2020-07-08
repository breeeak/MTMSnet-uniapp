<template>
	<view class="content">
		<!-- #ifndef MP -->
		<view class="status_bar">
			<!-- 这里是状态栏  空出-->
		</view>
		<!-- #endif -->
		<view class="solid-top padding   flex justify-between">
			<view class="action">
				<text class="cuIcon-title text-orange">检测进度</text>
			</view>
			<view>
				<text class="cuIcon-question text-green">操作帮助</text>
			</view>
		</view>
		<view class="padding solid-bottom">
			<view class="cu-steps">
				<view class="cu-item" :class="index>num?'':'text-blue'" v-for="(item,index) in numList" :key="index">
					<text class="num" :class="index==3?'err':''" :data-index="index + 1"></text> {{item.name}}
				</view>
			</view>
		</view>
		<view class="bg-white">
			<view class="solid-top solid-bottom padding  flex justify-between">
				<view class="action">
					<text class="cuIcon-picfill text-orange">原图</text>
				</view>
				<view>
					<text class="cuIcon-refresh text-green">重新选择</text>
				</view>
			</view>
			<view class="image">
				<image src="../../static/textileImg/20190218-213245-174.png" mode="widthFix" style="width: 750rpx;"></image>
			</view>
			<view class="solid-top solid-bottom padding flex justify-between">
				<view class="action">
					<text class="cuIcon-creativefill text-orange">识别结果</text>
				</view>
				<view>
					<text class="cuIcon-same text-green">查看中间结果</text>
				</view>
			</view>
			
			<view class="cu-form-group">
				<view class="title">经密:</view>
				<input placeholder="143 根/英寸" name="input"></input>
				<text class='cuIcon-locationfill text-green text-sm' style="font-size: 24rpx;">反馈错误</text>
			</view>
			<view class="cu-form-group">
				<view class="title">纬密:</view>
				<input placeholder="122 根/英寸" name="input"></input>
				<text class='cuIcon-locationfill text-green text-sm' style="font-size: 24rpx;">反馈错误</text>
			</view>
			<view class="cu-form-group">
				<view class="title">组织:</view>
				<input placeholder="" name="input"></input>
				<text class='cuIcon-locationfill text-green text-sm' style="font-size: 24rpx;">反馈错误</text>
			</view>
			<view class="image">
				<image src="../../static/textileImg/232332.png" mode="widthFix" style="width: 750rpx;"></image>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				numList: [{
					name: '选择图片'
				}, {
					name: '上传识别'
				}, {
					name: '识别结果'
				}, {
					name: '结果反馈'
				}, ],
				num: 2,
			}
		},
		onLoad() {

		},
		methods: {

		}
	}
</script>

<style lang="scss">
	.status_bar {
		height: $navigation-bar-height+40upx;
		width: 100%;
	}
</style>
