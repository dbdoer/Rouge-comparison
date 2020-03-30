<template>
	<view>
		<w-loading mask="false" click="true" ref="loading"></w-loading>
		<view class="cu-bar search header">
			<view class="search-form round my-shadow">
				<text class="cuIcon-search"></text>
				<input class="text-center" @focus="InputFocus" @blur="InputBlur" :adjust-position="true" type="text" :placeholder="brand" confirm-type="search"></input>
			</view>
			<view class="action">
				<button class="cu-btn bg-mine shadow-blur round">搜索</button>
			</view>
		</view>
		
		<view class="goods-list">
			<view class="product-list">
				<view class="product align-center" :key="index" v-for="(product, index) in productList" @tap="toGoods(product)">
					<view class="img align-center">
						<image mode="widthFix" :src="product.img"></image>
					</view>
					<view class="name">
						<view class="name1">{{ product.name1 }}{{ product.name2 }}</view>
						<view class="cu-tag bg-mine round">{{ product.name3 }}</view>
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
				brand: null,
				productList: [],
			}
		},
		created() { //此处用created相当于对前端页面数据进行初始化  
			var address = 'https://www.xiaoqw.online/nyz/server/smallList.php';
			this.brand = uni.getStorageSync('brandName');
			
			http.post(address, {
				enName: uni.getStorageSync('enName'),
				brand: uni.getStorageSync('brandName')
			}).then(res => {
				//这里是ES6的写法，get请求的地址	
				this.productList = res.data; //获取数据  
				console.log('success');
				console.log(this.productList);
				this.$refs.loading.close();
			})
		},
		onReady() {
			this.$refs.loading.open()
		},
		methods: {
			//商品跳转
			toGoods(e) {
				uni.setStorageSync('goodsImg', e.img);
				uni.setStorageSync('goodsName', e.name1+e.name2+e.name3);
				uni.setStorageSync('goodsName1', e.name1);
				uni.setStorageSync('goodsName3', e.name3);
				uni.navigateTo({
					url: "../goods/goods",
					animationType: 'pop-in',
					animationDuration: 200
				});
			}
		}
	}
</script>

<style lang="scss">
.header {
		background-color: #F5F6FA;
		position: fixed;
		top: auto;
		z-index: 10;
		width: 100%;
	}

	.goods-list {
		width: 100%;
		padding-top: 130rpx;
		padding-bottom: 50rpx;
	
		.product-list {
			width: 100%;
			padding: 0 4% 0 4%;
	
			.product {
				width: 100%;
				border-radius: 50rpx;
				display: flex;
				justify-content: space-between;
				background-color: #FFFFFF;
				margin-bottom: 20px;
	
				.img {
					width: 30%;
					padding: 4% 4%;
				}
	
				.name {
					width: 70%;
					font-size: 30rpx;
					text-align: center;
	
					.name1 {
						margin-bottom: 15px;
					}
				}
			}
		}
	}
</style>
