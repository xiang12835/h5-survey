<template>
	<view>
		<view class="uni-padding-wrap uni-common-mt">
			<uni-segmented-control :current="current" :values="items" :style-type="styleType" :active-color="activeColor"
			 @clickItem="onClickItem" />
		</view>
		<view class="content">
			<view v-show="current === 0">
				<view class="uni-list">
					<view class="uni-list-cell" hover-class="uni-list-cell-hover" v-for="(item,index) in newsList" :key="index" @tap="newsdetail"
					 :data-newsid="item.post_id">
						<view class="uni-media-list">
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
	import uniSegmentedControl from '@/components/uni-segmented-control/uni-segmented-control.vue'
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
				newsList: []
			}
		},
		methods: {
			onClickItem(index) {
				if (this.current !== index) {
					this.current = index
				}
			},
			newsdetail(e) {
				console.log(e);
				var newsid = e.currentTarget.dataset.newsid;
				console.log(newsid);
				uni.navigateTo({
					url: '../info/info?newsid=' + newsid,
				});
			}
		},
		onLoad: function() {
			uni.showLoading({
				title: "加载中..."
			})
			uni.request({
				url: 'https://unidemo.dcloud.net.cn/api/news',
				method: 'GET',
				data: {},
				success: res => {
					console.log(res.data);
					this.newsList = res.data;
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
