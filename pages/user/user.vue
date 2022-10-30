<template>
	<view class="user">
	<view class="top">
		<image src="../../static/images/time.png" mode=""></image>
		<view class="text">浏览历史</view>
	</view>
	<view class="content">
	<view class="row" v-for="item in listArr">
		<newsbox :item="item" @click.native="toDetail(item)"></newsbox>
	</view>
	</view>
	<view class="nothing" v-if="!listArr.length">
		<image src="../../static/images/nohis.png" mode="widthFix"></image>
		<view class="text">
			暂无浏览记录
		</view>
	</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				listArr:[]
			};
		},
		onShow(){
			this.getData()
		},
		methods:{
			//跳转到详情页面
			toDetail(item){
			uni.navigateTo({
				url:`/pages/detail/detail?cid=${item.classid}&id=${item.id}`
			})
			},
			//获取缓存浏览记录
			getData(){
				let listArr=uni.getStorageSync("historyArr") ||[]
				this.listArr=listArr
			}
		}
	}
</script>

<style lang="scss">
.user{
	.top{
		padding: 50rpx 0;
		background-color: #f8f8f8;
		height: 300rpx;
		display: flex;
		flex-direction: column;
		align-items: center;
		image{
			width: 150rpx;
			height: 150rpx;
		}
		.text{
			font-size: 38rpx;
			color: #666;
			padding-top: 20rpx;
		}
	}
	.content{
		padding: 30rpx;
		.row{
			border-bottom: 1px dotted #efefef;
			padding:20rpx 0;
		}
	}
	.nothing{
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		images{
			max-width: 450rpx;;
		}
		.text{
			font-size: 26rpx;
			color: #888;
		}
	}
}
</style>
