<template>
	<view>
		<!-- <w-loading mask="false" click="true" ref="loading"></w-loading> -->
		<view class="picture">
			<image class="shadow-blur round" mode="widthFix" :src="img"></image>
		</view>
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
						<view v-if="this.jdInfo !== null" class="price">￥{{ jdInfo.price }}</view>
						<view v-if="this.jdInfo === null" class="price">暂无</view>
					</view>
					<button class="background-right cu-btn round align-center" @tap="toShop(good)">
						<view class="name">京东 TO SHOP</view>
					</button>
				</view>
			</view>
			<view class="platform-list">
				<view class="platform align-center">
					<view class="background-left align-center">
						<view v-if="this.tmInfo !== null" class="price">￥{{ tmInfo.price }}</view>
						<view v-if="this.tmInfo === null" class="price">暂无</view>
					</view>
					<button class="background-right cu-btn round align-center" @tap="toShop(good)">
						<view class="name">天猫 TO SHOP</view>
					</button>
				</view>
			</view>
			<view class="platform-list">
				<view class="platform align-center">
					<view class="background-left align-center">
						<view class="price">暂无</view>
					</view>
					<button class="background-right cu-btn round align-center" @tap="toShop(good)">
						<view class="name">小红书 TO SHOP</view>
					</button>
				</view>
			</view>
			<view class="platform-list">
				<view class="platform align-center">
					<view class="background-left align-center">
						<view class="price">暂无</view>
					</view>
					<button class="background-right cu-btn round align-center" @tap="toShop(good)">
						<view class="name">拼多多 TO SHOP</view>
					</button>
				</view>
			</view>
		</view>
		<view style="height: 50rpx;"></view>
	</view>
	</view>
</template>

<script>
	import Vuex from "vuex";
	import axios from "axios";
	import http from '@/utils/http.js'

	export default {
		data() {
			return {
				jdInfo: null,
				tmInfo: null,
				name: null,
				img: null,
				collectIcon: null,
			}
		},
		created() { //此处用created相当于对前端页面数据进行初始化  
			this.name = uni.getStorageSync('goodsName');
			this.img = uni.getStorageSync('goodsImg');
			
			// var value = '%' + uni.getStorageSync('goodsName1') + '%' + uni.getStorageSync('goodsName3') + '%';
			var address = 'http://120.55.87.80/server/Goods/Goods.php';

			http.post(address, {
				enName: uni.getStorageSync('enName'),
				brand: '京东',
				value: '%' + uni.getStorageSync('goodsName1') + '%' + uni.getStorageSync('goodsName3') + '%'
			}).then(res => {
				this.jdInfo = res.data; //获取数据  
				console.log('success');
				console.log(this.jdInfo);
				// this.$refs.loading.close();
			}),
			http.post(address, {
				enName: uni.getStorageSync('enName'),
				brand: '天猫',
				value: '%' + uni.getStorageSync('goodsName1') + '%' + uni.getStorageSync('goodsName3') + '%'
			}).then(res => {
				this.tmInfo = res.data; //获取数据  
				console.log('success');
				console.log(this.tmInfo);
				// this.$refs.loading.close();
			})
		},
		onReady() {
			// this.$refs.loading.open();
			this.collectIcon = 'cuIcon-like'
		},
		methods: {
			toCollect() {
				if(this.collectIcon == 'cuIcon-like') {
					this.collectIcon = 'cuIcon-likefill'
				}
				else if(this.collectIcon == 'cuIcon-likefill') {
					this.collectIcon = 'cuIcon-like'
				}
			},
			toShop(e) {
				uni.setStorageSync('webUrl', e.address);
				uni.navigateTo({
					url: '../shopweb/shopweb',
					animationType: 'pop-in',
					animationDuration: 200
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
		width: 90%;
		margin: 0 auto;
		justify-content: center;

		image {
			width: 100%;
		}
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

		.platform-list {
			width: 100%;
			padding: 0 4% 0 4%;
			justify-content: space-between;

			.platform {
				width: 100%;
				height: 150rpx;
				border-radius: 50rpx;
				display: flex;
				background-color: #FFFFFF;
				margin: 0 0 50rpx 0;

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
					width: 60%;
					height: 90rpx;
					margin-right: 30rpx;
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
