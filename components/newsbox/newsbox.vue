<template>
	<view class="newsbox">
		<view class="img">
			<image :src="item.picurl" mode="aspectFill"></image>
		</view>
		<view class="content">
			<view class="title">
				{{item.title}}
			</view>
			<view class="info" v-if="!item.looktime">
				<text>{{item.author}}</text>
				<text>{{item.hits}}</text>
			</view>
			<view class="info" v-else>
				<text>浏览时间: {{item.looktime}}</text>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		name:"newsbox",
		props:{
			item:{
				type:Object,
				default(){
					return{
						title:'组件默认标题',
						author:'张三',
						hits:'669',
						picurl:'../../static/images/p01.jpeg',
					    looktime:'2021'
					}
				}
			}
		},
		data() {
			return {
				
			};
		}
	}
</script>

<style lang="scss">
	.newsbox{
		display: flex;
		.img{
			width: 230rpx;
			height: 160rpx;
			image{
				width: 100%;
				height: 100%;
			}
		}
		.content{
			flex:1;
			padding-left:20rpx;
			display: flex;
			flex-direction: column;
			justify-content: space-between;
			.title{
				font-size: 36rpx;
				color:#333 ;
				  overflow: hidden;
				  text-overflow: ellipsis;
				  display: -webkit-box;
				  -webkit-line-clamp: 2;
				  -webkit-box-orient: vertical;

			}
			.info{
				font-size: 26rpx;
				color: #999;
				text{
					padding-right: 30rpx;
				}
			}
		}
	}
</style>