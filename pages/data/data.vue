<template>
	<view class="data">
		<header-bar :isBack="isBack" :title="i18n.tabbar.data"></header-bar>
		<view class="content">
			<swiper class="swiper-con" :indicator-dots="true" :autoplay="true">
				<swiper-item>
					<view class="swiper-item bg2">
						<view class="swiper-top">
							<image src="../../static/images/icon20.png"></image>
							<text>{{i18n.data.lang32}}</text>
						</view>
						<view class="swiper-bot">
							<text class="big">{{dataList.pool?dataList.pool.total:0.00}}</text>
							<text class="small">BWK</text>
						</view>
					</view>
				</swiper-item>
				<swiper-item>
					<view class="swiper-item bg1">
						<view class="swiper-top">
							<image src="../../static/images/icon21.png"></image>
							<text>{{i18n.data.lang33}}</text>
						</view>
						<view class="swiper-bot">
							<text class="big">{{dataList.pool?dataList.pool.market:0.00}}</text>
							<text class="small">BWK</text>
						</view>
					</view>
				</swiper-item>
				<swiper-item>
					<view class="swiper-item bg3">
						<view class="swiper-top">
							<image src="../../static/images/icon22.png"></image>
							<text>{{i18n.data.lang34}}</text>
						</view>
						<view class="swiper-bot">
							<text class="big">{{dataList.pool?dataList.pool.released:0.00}}</text>
						</view>
					</view>
				</swiper-item>
			</swiper>
			<view class="count-wrap">
				<view class="tab-top">
					<view class="list1" :class="{'active': current == 1}" @tap="changeTab(1)"></view>
					<view class="list2" :class="{'active': current == 2}" @tap="changeTab(2)"></view>
				</view>
				<view class="tab-con1" v-show="current == 1">
					<view class="con1-list">
						<view class="left">
							<text>{{i18n.data.lang36_1}}</text>
						</view>
						<view class="right">
							<text>{{dataList.community?dataList.community.week:0.00}}BWK</text>
						</view>
					</view>
					<view class="con1-list small">
						<view class="left">
							<text>{{i18n.data.data1}}{{dataList.day}}{{i18n.data.data2}}</text>
						</view>
						<view class="right" @tap="gotoPage('level')">
							<text>{{i18n.data.data3}}{{dataList.level}}</text>
						</view>
					</view>
					<!-- <view class="con1-list">
						<view class="left">
							<text>{{i18n.data.lang36}}</text>
						</view>
						<view class="right">
							<text>{{dataList.community?dataList.community.month:0.00}}</text>
						</view>
					</view>
					<view class="con1-list">
						<view class="left">
							<text>{{i18n.data.lang37}}</text>
						</view>
						<view class="right">
							<text>{{dataList.community?dataList.community.quarter:0.00}}</text>
						</view>
					</view> -->
					<view class="number-wrap">
						<view class="number-item">
							<view class="left">{{i18n.data.lang38}}:</view>
							<view class="right">{{dataList.team?dataList.team.poolTotal:0.00}}</view>
						</view>
						<view class="operation">
							<view class="operation-btn red" @tap="getPrize('getWeekPrize')">
								<text :class="{'isget': dataList.award && dataList.award.is_week == 1}">{{i18n.data.lang41_1}}</text>
							</view>
							<view class="operation-btn yel" @tap="getPrize('getMonthPrize')">
								<text :class="{'isget': dataList.award && dataList.award.is_month == 1}">{{i18n.data.lang41}}</text>
							</view>
							<view class="operation-btn blu" @tap="getPrize('getQuartePrize')">
								<text :class="{'isget': dataList.award && dataList.award.is_quarer == 1}">{{i18n.data.lang42}}</text>
							</view>
						</view>
					</view>
					<!-- <view class="hr"></view>
					<view class="con1-list big">
						<view class="left">
							<text>{{i18n.data.data4}}</text>
						</view>
						<view class="right">
							<text>{{dataList.superNode?dataList.superNode.amount:0.00}}USDT</text>
						</view>
						<view class="btn" :class="{'change': dataList.superNode && dataList.superNode.status != -1}" @tap="receive(dataList.superNode.status)">
							<text>{{i18n.data.data5}}</text>
						</view>
						<view class="status" :class="{'handing': dataList.superNode && dataList.superNode.status == 0, 
														'success': dataList.superNode && dataList.superNode.status == 1,
														'fail': dataList.superNode && dataList.superNode.status == 2,} ">
							<text>{{dataList.superNode?i18n.data['dataStatus'+ dataList.superNode.status]:''}}</text>
						</view>
					</view> -->
				</view>
				<view class="tab-con2" v-show="current == 2">
					<view class="tab2-list" v-for="(item, index) in teamsMill" :key="index">
						<view class="time">
							<text>{{item.create_time}}（{{i18n.index.mining19}}{{item.endDay}}{{i18n.index.mining20}}）</text>
						</view>
						<view class="stutas">
							<view class="top">
								<text>{{item.name}}</text>
								<text>{{i18n.index.mining21}}</text>
							</view>
							<view class="bot">
								<text>{{item.actual_money}}USDT</text>
								<text>{{i18n.index['mine' + item.status]}}</text>
							</view>
						</view>
						<view class="hr2"></view>
						<view class="user">
							<view class="top">
								<text>{{i18n.index.mining22}}</text>
								<text>{{i18n.index.mining23}}</text>
							</view>
							<view class="bot">
								<text>{{item.phone}}({{item.level}}{{i18n.index.mining24}})</text>
								<text>{{item.performance}}USDT</text>
							</view>
						</view>
					</view>
					<view class="nodata" v-if="teamsMill.length == 0">{{i18n.history.history17}}</view>
				</view>
			</view>
			<!-- <swiper class="swiper" autoplay="autoplay">
				<swiper-item v-for="(item, index) in slideList" :key="index">
					<image :src="item.pic" mode="scaleToFill"></image>
				</swiper-item>
			</swiper> -->
		</view>
		<tabbar :isCurrent="2"></tabbar>
	</view>
</template>

<script>
	import HeaderBar from '../../components/header-bar.vue'
	import Tabbar from '../../components/tabbar.vue'
	export default {
		data() {
			return {
				isBack: false,
				dataList: [],
				slideList: [],
				current: 1,
				teamsMill: [],
			}
		},
		components: {
			HeaderBar,
			Tabbar
		},
		onLoad() {
			this.getData()
			// this.getSlideData()
		},
		methods: {
			getData(){
				this.uniRequest({
					url: 'performance',
					method: 'GET'
				}).then(res => {
					this.dataList = res.result
				})
				this.uniRequest({
					url: 'teamsMill'
				}).then(res => {
					this.teamsMill = res.result
				})
			},
			getSlideData(){
				this.uniRequest({
					url: 'slideshow',
					method: 'GET'
				}).then(res => {
					this.slideList = res.result
				})
			},
			gotoPage(url){
				uni.navigateTo({
				    url
				});
			},
			getPrize(url){
				// 获取月奖励
				this.uniRequest({
					url,
					method: 'GET'
				}).then(res => {
					uni.showToast({
						title: res.message,
						icon: 'none',
						success: () => {
							setTimeout(() => {
								this.getData()
							}, 2000)
						}
					})
				})
			},
			receive(status){
				//领取超级节点
				if(status>-1) return false
				this.uniRequest({
					url: 'getSuperNode',
					method: 'GET'
				}).then(res => {
					uni.showToast({
						title: res.message,
						icon: 'none',
						success: () => {
							setTimeout(() => {
								this.getData()
							}, 2000)
						}
					})
				})
			},
			changeTab(status){
				this.current = status
			}
		},
		computed: {  
			i18n () {  
				return this.$t('index')  
			}  
		}
	}
</script>

<style lang="scss">
/* #ifdef H5 */
uni-page-body{
	padding-bottom: 100rpx;
}
/* #endif */
.data{
	min-height: 100%;
	.content {
		width: 100%;
		min-height: 100%;
		box-sizing: border-box;
		padding: 0 30rpx 30rpx;
		background-size: contain;
		.swiper-con{
			margin-top: 28rpx;
			.swiper-item{
				height: 300rpx;
				padding: 30rpx 30rpx 0;
				border-radius: 16rpx;
				box-sizing: border-box;
				&.bg1{
					background: url(../../static/images/swiper.png) no-repeat;
					background-size: cover;
				}
				&.bg2{
					background: url(../../static/images/swiper2.png) no-repeat;
					background-size: cover;
				}
				&.bg3{
					background: url(../../static/images/swiper3.png) no-repeat;
					background-size: cover;
				}
				.swiper-top{
					font-size: 36rpx;
					color: #FFFFFF;
					display: flex;
					align-items: center;
					image{
						width: 48rpx;
						height: 48rpx;
						margin-right: 10rpx;
					}
				}
				.swiper-bot{
					color: #FFFFFF;
					text-align: center;
					margin-top: 50rpx;
					.big{
						font-size: 52rpx;
					}
					.small{
						font-size: 42rpx;
					}
				}
			}
		}
		.swiper{
			margin-top: 40rpx;
			image{
				width: 100%;
				height: 100%;
			}
		}
		.data-wrap{
			display: flex;
			margin: 0 -10rpx;
			.data-wrap-item{
				width: 223.3333rpx;
				flex: 1;
				background-color: #34374D;
				border-radius: 8rpx;
				margin: 0 10rpx;
				padding: 24rpx 0;
				overflow: hidden;
				.top{
					display: flex;
					align-items: center;
					justify-content: center;
				}
				image{
					width: 40rpx;
					height: 40rpx;
					margin-right: 16rpx;
				}
				.label{
					font-size: $fontI;
					color: $colorA;
				}
				.count{
					font-size: 28rpx;
					color: #59D0CF;
					margin-left: 30rpx;
					text-align: center;
				}
			}
		}
		.count-wrap{
			margin-top: 52rpx;
			.tab-top{
				display: flex;
				align-items: center;
				.list1{
					width: 158rpx;
					height: 42rpx;
					background: url(../../static/images/icon60_1.png) no-repeat;
					background-size: contain;
					margin-right: 70rpx;
					&.active{
						background: url(../../static/images/icon60.png) no-repeat;
						background-size: contain;
						position: relative;
						&::before{
							content: "";
							display: block;
							width: 70rpx;
							height: 6rpx;
							background-color: #59D0CF;
							position: absolute;
							bottom: -10rpx;
							left: 50%;
							margin-left: -35rpx;
						}
					}
				}
				.list2{
					width: 232rpx;
					height: 42rpx;
					background: url(../../static/images/icon61_1.png) no-repeat;
					background-size: contain;
					&.active{
						background: url(../../static/images/icon61.png) no-repeat;
						background-size: contain;
						position: relative;
						&::before{
							content: "";
							display: block;
							width: 70rpx;
							height: 6rpx;
							background-color: #59D0CF;
							position: absolute;
							bottom: -10rpx;
							left: 50%;
							margin-left: -35rpx;
						}
					}
				}
			}
			.tab-con1{
				margin-top: 60rpx;
				.con1-list{
					line-height: 112rpx;
					background-color: #34374D;
					display: flex;
					justify-content: space-between;
					align-items: center;
					padding: 0 30rpx;
					border-radius: 8rpx;
					margin-bottom: 28rpx;
					&.small{
						line-height: 70rpx;
						background-color: #34374D;
						margin-top: -12rpx;
						.left{
							font-size: 24rpx;
							color: #59D0CF;
						}
						.right{
							font-size: 24rpx;
							color: #FFFFFF;
						}
					}
					&.big{
						flex-wrap: wrap;
						.left{
							width: 20%;
						}
						.right{
							width: 80%;
							text-align: right;
						}
						.btn{
							width: 236rpx;
							height: 80rpx;
							line-height: 80rpx;
							border-radius: 40rpx;
							background-color: #59D0CF;
							font-size: 32rpx;
							color: #FFFFFF;
							text-align: center;
							margin-bottom: 24rpx;
							&.change{
								opacity: 0.5;
							}
						}
						.status{
							font-size: 28rpx;
							text-align: right;
							&.success{
								color: #59D0CF;
							}
							&.fail{
								color: #D8575A;
							}
							&.handing{
								color: #FFFFFF;
							}
						}
					}
					.left{
						font-size: 28rpx;
						color: #999999;
					}
					.right{
						font-size: 42rpx;
						color: #59D0CF;
					}
				}
			}
			.tab-con2{
				margin-top: 60rpx;
				.nodata{
					font-size: 32rpx;
					color: #c7c7c7;
					text-align: center;
					line-height: 100rpx;
				}
				.tab2-list{
					background-color: #34374D;
					border-radius: 8rpx;
					padding: 30rpx;
					margin-bottom: 24rpx;
					box-shadow: 0rpx 8rpx 8rpx 0rpx rgba(0, 0, 0, 0.05), 0rpx -2rpx 0rpx 0rpx #272A3F;
					.time{
						font-size: 24rpx;
						color: #FFFFFF;
						margin-bottom: 16rpx;
					}
					.stutas, .user{
						font-size: 24rpx;
						color: #FFFFFF;
						.top, .bot{
							display: flex;
							justify-content: space-between;
							align-items: center;
						}
						.bot{
							margin-top: 8rpx;
							text:first-child{
								font-size: 36rpx;
								color: #D8575A;
							}
							text:last-child{
								font-size: 32rpx;
							}
						}
					}
					.hr2{
						margin: 24rpx 0;
						height: 0;
						border-top: 1px dashed #454545;
					}
					.user{
						.bot{
							text:first-child{
								color: #FFFFFF;
							}
							text:last-child{
								color: #59D0CF;
							}
						}
					}
				}
			}
			.subtitle{
				width: 228rpx;
				height: 44rpx;
				margin: 0 auto;
				background: url('../../static/images/icon23_1.png') no-repeat;
				background-size: contain;
			}
			.hr{
				height: 2rpx;
				background: url('../../static/images/icon19.png') bottom repeat-x;
				margin: 44rpx 0;
			}
			.number-wrap{
				background-color: #59D0CF;
				border-radius: 8rpx;
				padding: 40rpx 30rpx 46rpx;
				box-sizing: border-box;
				.number-item{
					.left{
						font-size: 32rpx;
						color: #2F2F2F;
					}
					.right{
						font-size: 48rpx;
						color: #D8575A;
					}
				}
			}
			.operation{
				margin-top: 40rpx;
				display: flex;
				justify-content: space-between;
				align-items: center;
				padding: 0 20rpx;
				.operation-btn{
					width: 40%;
					height: 112rpx;
					line-height: 112rpx;
					text-align: center;
					font-size: $fontG;
					color: $colorA;
					border-radius: 8rpx;
					margin-left: 20rpx;
					&:first-child{
						margin-left: 0;
					}
					&.red{
						background-color: #D8575A;
					}
					&.yel{
						background-color: #D5A366;
					}
					&.blu{
						background-color: #0E5CCE;
					}
				}
				.isget{
					position: relative;
					&::after{
						position: absolute;
						content: "";
						display: block;
						width: 12rpx;
						height: 12rpx;
						border-radius: 50%;
						background-color: #FF4436;
						right: -6rpx;
						top: 4rpx;
					}
				}
			}
		}
	}
}
</style>
