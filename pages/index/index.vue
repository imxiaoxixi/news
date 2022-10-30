<template>
<view class="home">
	<scroll-view  class="navscroll" scroll-x >
		<view class="item " :class="index==NavIndex? 'active':''" v-for="(item,index) in NavArr" :key='item.id' 
		@click="clickNav(index,item.id)">{{item.classname}}</view>
	</scroll-view>
	<view class="content">
		<div class="row" v-for="item in NewsArr" :key="item.id">
			<newsbox :item="item" @click.native="toDetail(item)"></newsbox>
		</div>
	</view>
	<view class="nodata" v-if="!NewsArr.length">
		<image src="../../static/images/nodata.png" mode="widthFix"></image>
	</view>
	<view class="loading" v-if="NewsArr.length">
		<view v-if="loading==1">数据加载中...</view>
		<view v-if="loading==2">没有更多了~</view>
	</view>
</view>
</template>

<script>
	export default {
		data() {
			return {
				title: 'Hello',
				NavIndex:0,
				NavArr:[],
				NewsArr:[],
				currentPage:1,
				loading:0,    //0默认  1 加载中  2 没有更多
			}
		},
		onLoad() {
			this.getNavData()
			this.getNewsData()
		},
		//触底加载
		onReachBottom(){
			if(this.loading=2){
				return;
			}
			this.currentPage++;
			this.loading=1;
			this.getNewsData()
		},
		methods: {
			//点击导航切换
			clickNav(index,id){
				this.NavIndex=index
				this.currentPage=1
				this.NewsArr=[]
				this.loading=0
				this.getNewsData(id)
			},
			//跳转到详情页面
			toDetail(item){
			uni.navigateTo({
				url:`/pages/detail/detail?cid=${item.classid}&id=${item.id}`
			})
			},
			//获取导航列表数据
			getNavData(){
				uni.request({
					url:"https://ku.qingnian8.com/dataApi/news/navlist.php",
					success:res=>{
						this.NavArr=res.data
					}
				})
			},
			//获取新闻详情
			getNewsData(id=50){
				uni.request({
					url:"https://ku.qingnian8.com/dataApi/news/newslist.php",
					data:{
						cid:id,
						page:this.currentPage,
					},
					success:res=>{
						if(res.data.length==0){
							this.loading=2
						}
						this.NewsArr=[...this.NewsArr,...res.data]
					}
				})
			}
		}
	}
</script>

<style lang="scss" scoped>
.navscroll{
	height: 100rpx;
	background-color: #f5f7f9;
	white-space: nowrap;
	position: fixed;
	top: var(--window-top);
	left: 0;
	z-index: 10;
	/deep/ ::-webkit-scrollbar{
		width: 4px !important;
		height: 1px !important;
		overflow: auto !important;
		background: transparent !important;
		-webkit-appearance: auto !important;
		display: block;
	}
}
.item{
	font-size: 40rpx;
	color: #333;
	display: inline-block;
	line-height: 100rpx;
	padding: 0 30rpx;
	&.active{
		color:#4ECDC4 ;
	}
}
.content{
	padding: 30rpx;
	padding-top: 130rpx;
	.row{
		border-bottom: 1px dotted #efefef;
		padding:20rpx 0;
	}
}
	.nodata{
		display: flex;
		justify-content: center;
		image{
			width: 360rpx;
		}
	}
	.loading{
		font-size: 26rpx;
		color: #888;
		text-align: center;
		line-height: 2em;
	}
</style>
