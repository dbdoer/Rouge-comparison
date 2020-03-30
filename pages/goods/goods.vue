<template>
	<view>
		<image class="picture shadow-blur" mode="widthFix" :src="img"></image>

		<view class="collection align-center">
			<view class="name">{{ name }}</view>
			<view class="icon-box">
				<button class="icon-round cu-btn size" :class="collectIcon" @tap="toCollect()"></button>
			</view>
		</view>

		<view class="platform-box">
			<view class="platform-list">
				<view class="platform align-center">
					<view class="background-left align-center">
						<view v-if="jdInfo.price" class="price">￥{{ jdInfo.price }}</view>
						<view v-if="!jdInfo.price" class="price">暂无</view>
					</view>
					<button class="background-right cu-btn round align-center" @tap="paste(jdInfo.address)">
						<view class="name">京东 TO SHOP</view>
					</button>
				</view>
			</view>
			<view class="platform-list">
				<view class="platform align-center">
					<view class="background-left align-center">
						<view v-if="tmInfo.price" class="price">￥{{ tmInfo.price }}</view>
						<view v-if="!tmInfo.price" class="price">暂无</view>
					</view>
					<button class="background-right cu-btn round align-center" @tap="paste(tmInfo.address)">
						<view class="name">天猫 TO SHOP</view>
					</button>
				</view>
			</view>
			<view class="platform-list">
				<view class="platform align-center">
					<view class="background-left align-center">
						<view class="price">暂无</view>
					</view>
					<button class="background-right cu-btn round align-center">
						<view class="name">小红书 TO SHOP</view>
					</button>
				</view>
			</view>
			<view class="platform-list">
				<view class="platform align-center">
					<view class="background-left align-center">
						<view class="price">暂无</view>
					</view>
					<button class="background-right cu-btn round align-center">
						<view class="name">拼多多 TO SHOP</view>
					</button>
				</view>
			</view>
		</view>
		<view style="height: 80rpx; background-color: #f7f2f0;"></view>
	</view>
	</view>
</template>

<script>
	import Vuex from "vuex";
	import axios from "axios";
	import http from '@/utils/http.js';

	export default {
		data() {
			return {
				jdInfo: [],
				tmInfo: [],
				name: null,
				img: null,
				collectIcon: null,
			}
		},
		created() { //此处用created相当于对前端页面数据进行初始化  
			this.name = uni.getStorageSync('goodsName');
			this.img = uni.getStorageSync('goodsImg');

			var address = 'https://www.xiaoqw.online/nyz/server/Goods.php';
			http.post(address, {
					enName: uni.getStorageSync('enName'),
					brand: '京东',
					goodsName: '%' + uni.getStorageSync('goodsName1') + '%' + uni.getStorageSync('goodsName3') + '%'
				}).then(res => {
					this.jdInfo = res.data; //获取数据
					console.log('success');
					console.log(this.jdInfo);
				}),

				http.post(address, {
					enName: uni.getStorageSync('enName'),
					brand: '天猫',
					goodsName: '%' + uni.getStorageSync('goodsName1') + '%' + uni.getStorageSync('goodsName3') + '%'
				}).then(res => {
					this.tmInfo = res.data; //获取数据
					console.log('success');
					console.log(this.tmInfo);
				});
		},
		onReady() {
			// this.$refs.loading.open();
			this.collectIcon = 'cuIcon-like'
		},
		methods: {
			toCollect() {
				if (this.collectIcon == 'cuIcon-like') {
					this.collectIcon = 'cuIcon-likefill'
				} else if (this.collectIcon == 'cuIcon-likefill') {
					this.collectIcon = 'cuIcon-like'
				}
			},
			paste(value) {
				uni.setClipboardData({
					data: value,
					success: function() {
						console.log('success');
						uni.showToast({
							title: '链接已复制',
							icon: 'none'
						});
					}
				});
			}
		}
	}
</script>

<style lang="scss">
	.header {
		background-color: #F8F8F8;
		position: fixed;
		top: auto;
		z-index: 10;
		width: 100%;
	}

	.picture {
		width: 100%;
		border-bottom-left-radius: 25px;
		border-bottom-right-radius: 25px;
	}

	.collection {
		width: 100%;
		display: flex;
		margin-top: 70rpx;
		margin-bottom: 70rpx;

		.name {
			width: 55%;
			margin-left: 10%;
			font-weight: 600;
			font-size: 35rpx;
		}

		.icon-box {
			width: 35%;

			.icon-round {
				width: 100rpx;
				height: 100rpx;
				margin-left: 35%;
				border-radius: 50rpx;
				background-color: #FFFFFF;
			}

			.size {
				font-size: 40rpx;
			}
		}
	}

	.platform-box {
		width: 100%;
		justify-content: center;
		background-color: #f7f2f0;
		border-top-left-radius: 25px;
		border-top-right-radius: 25px;

		.platform-list {
			width: 100%;
			padding-top: 20px;
			justify-content: space-between;

			.platform {
				width: 100%;
				height: 150rpx;
				border-radius: 50rpx;
				display: flex;

				.background-left {
					width: 40%;
					height: 120rpx;
					font-size: 35rpx;

					.price {
						margin: 0 auto;
						text-align: center;
						margin-top: 38rpx;
					}
				}

				.background-right {
					width: 50%;
					height: 90rpx;
					text-align: center;
					background-color: #CF6C7E;

					.name {
						text-align: center;
						color: #FFFFFF;
						font-size: 35rpx;
					}
				}
			}
		}
	}
</style>
