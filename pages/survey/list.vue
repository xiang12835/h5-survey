<template>
	<view>
		<view class="uni-padding-wrap uni-common-mt">
			<uni-segmented-control :current="current" :values="items" :style-type="styleType" :active-color="activeColor"
			 @clickItem="onClickItem" />
		</view>
		<view class="content">
			<view v-show="current === 0">
				<view class="uni-list">
					<view class="uni-list-cell" hover-class="uni-list-cell-hover" v-for="(item,index) in surveyList" :key="index" @tap="surveyDetail"
					 :data-surveyid="item.post_id">
						<view class="uni-media-list">
							<!-- <image class="uni-media-list-logo" :src="item.author_avatar"></image> -->
							<view class="uni-media-list-body">
								<view class="uni-media-list-text-top">{{item.title}}</view>
								<view class="uni-media-list-text-bottom uni-ellipsis">{{item.created_at}}</view>
							</view>
						</view>
					</view>
				</view>
			</view>
			<view v-show="current === 1">选项卡2的内容</view>
		</view>


	</view>
</template>

<script>
	import uniSegmentedControl from '@/components/uni-segmented-control/uni-segmented-control.vue';
	import apiUrl from '../../common/apiUrl.js'; 
	export default {
		components: {
			uniSegmentedControl
		},
		data() {
			return {
				items: ['反馈', '投票'],
				styles: [{
						value: 'button',
						text: '按钮',
						checked: true
					},
					{
						value: 'text',
						text: '文字'
					}
				],
				colors: ['#007aff', '#4cd964', '#dd524d'],
				current: 0,
				colorIndex: 0,
				activeColor: '#4cd964',
				styleType: 'button',
				surveyList: []
			}
		},
		methods: {
			onClickItem(index) {
				console.log(index);
				if (this.current !== index) {
					this.current = index
				}
			},
			surveyDetail(e) {
				console.log(e);
				var surveyid = e.currentTarget.dataset.surveyid;
				console.log(surveyid);
				uni.navigateTo({
					url: '../survey/detail?surveyid=' + surveyid,
				});
			}
		},
		onLoad: function() {
			uni.showLoading({
				title: "加载中..."
			})
			uni.request({
				url: apiUrl.surveyListApi,
				method: 'GET',
				data: {},
				success: res => {
					console.log(res.data);
					this.surveyList = res.data;
					uni.hideLoading();
				},
				fail: () => {},
				complete: () => {}
			});
		},
	}
</script>

<style>
	page {
		display: flex;
		flex-direction: column;
		box-sizing: border-box;
		background-color: #efeff4
	}

	view {
		font-size: 28upx;
		line-height: inherit
	}

	.uni-common-mt {
		margin-top: 20upx;
		margin-bottom: 20upx;
	}

	.uni-media-list-body {
		height: auto;
	}

	.uni-media-list-text-top {
		line-height: 1.6em;
	}
</style>
