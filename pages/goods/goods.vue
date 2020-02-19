<template>
	<view>
		<w-loading mask="false" click="true" ref="loading"></w-loading>
		<view :key="good.price" v-for="good in goods" style="width: 100%;">
			<view class="picture">
				<image class="shadow-blur round" mode="widthFix" :src="img"></image>
			</view>
			<view class="collection align-center">
				<view class="name">{{ name }}</view>
				<view class="icon-box">
					<button class="icon-round cu-btn cuIcon-like lg"></button>
				</view>
			</view>

			<view class="platform-box">
				<view class="platform-list">
					<view class="platform align-center">
						<view class="background-left align-center">
							<view class="price">￥{{ good.price }}</view>
						</view>
						<button class="background-right cu-btn round align-center" @tap="toShop(good)">
							<view class="name">{{ good.brand }} TO SHOP</view>
						</button>
					</view>
				</view>
			</view>
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
				goods: [],
				name: null,
				img: null,
			}
		},
		created() { //此处用created相当于对前端页面数据进行初始化  
			var goodsName = uni.getStorageSync('goodsName');
			var goodsImg = uni.getStorageSync('goodsImg');
			var value = '%' + uni.getStorageSync('goodsName1') + '%' + uni.getStorageSync('goodsName3') + '%';
			var address = 'http://120.55.87.80/server/Goods/' + uni.getStorageSync('enName') + 'Goods.php';

			this.name = goodsName;
			this.img = goodsImg;
			http.post(address, value).then(res => {
				//这里是ES6的写法，get请求的地址
				this.goods = res.data; //获取数据  
				console.log('success');
				console.log(this.goods);
				this.$refs.loading.close();
			})
		},
		onReady() {
			this.$refs.loading.open()
		},
		methods: {
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
				margin: 0 0 30rpx 0;
				
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
