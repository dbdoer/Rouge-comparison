<template>
	<view>
		<w-loading mask="false" click="true" ref="loading"></w-loading>
		
		<block v-if="searchData.length >= 0">
			<view class="goods-list">
				<view class="product-list">
					<view class="product align-center" :key="index" v-for="(product, index) in searchData" @tap="toGoods(product)">
						<view class="img align-center">
							<image mode="widthFix" :src="product.img"></image>
						</view>
						<view class="name">{{ product.name1 }}{{ product.name2 }}{{ product.name3 }}</view>
					</view>
				</view>
			</view>
		</block>
		
		<block v-if="searchData.length <= 0 && searchText != ''">
			<view class="result">暂无结果，您可以换一个搜索词</view>
		</block>
		
		<block v-if="searchText == ''">
			<view style="margin-top: -60rpx;"></view>
			<view class="goods-list">
				<view class="product-list">
					<view class="product align-center" :key="index" v-for="(product, index) in productList" @tap="toGoods(product)">
						<view class="img align-center">
							<image mode="widthFix" :src="product.img"></image>
						</view>
						<view class="name">{{ product.name1 }}{{ product.name2 }}{{ product.name3 }}</view>
					</view>
				</view>
			</view>
		</block>
	</view>
</template>

<script>
	import Vuex from "vuex";
	import axios from "axios";
	import http from '@/utils/http.js'

	export default {
		data() {
			return {
				searchText: '',
				productList: [],
				searchData: [],
			}
		},
		created() { //此处用created相当于对前端页面数据进行初始化  
			this.searchText = uni.getStorageSync('searchText');

			var address = 'http://120.55.87.80/server/search/search.php';
			http.post(address).then(res => {
				//这里是ES6的写法，get请求的地址
				this.productList = res.data; //获取数据  
				console.log('success');
				console.log(this.productList);
				this.Search();
				this.$refs.loading.close();
			});
			
		},
		onReady() {
			this.$refs.loading.open()
		},
		methods: {
			//商品跳转
			toGoods(e) {
				uni.setStorageSync('goodsImg', e.img);
				uni.setStorageSync('goodsName', e.name1 + e.name2 + e.name3);
				uni.setStorageSync('goodsName1', e.name1);
				uni.setStorageSync('goodsName3', e.name3);
				uni.navigateTo({
					url: "../goods/goods",
					animationType: 'pop-in',
					animationDuration: 200
				});
			},
			Search() {
				var search = this.searchText;
				if (search) {
					const result = [];
					for (let i = 0; i < this.productList.length; i++) {
						var product = this.productList[i].name1 + this.productList[i].name2 + this.productList[i].name3;
						if (String(product).toLowerCase().indexOf(search) > -1) {
							result.push(this.productList[i]);
						}
					}
					this.searchData = result;
				};
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
	
	.result {
		width: 100%;
		height: 60rpx;
		font-size: 34rpx;
		font-weight: 600;
		text-align: center;
		margin-bottom: 3%;
		color: #CF6C7E;
	}
	
	.goods-list {
		width: 100%;
		padding-top: 20rpx;
		padding-bottom: 50rpx;
		justify-content: center;

		.product-list {
			width: 100%;
			padding: 0 4% 0 4%;
			justify-content: space-between;

			.product {
				width: 100%;
				border-radius: 50rpx;
				display: flex;
				justify-content: space-between;
				background-color: #FFFFFF;
				margin: 0 0 30rpx 0;
				// box-shadow: 0px 0px 10px 0px #d0d0d0;

				.img {
					width: 30%;
					padding: 4% 5%;

					image {
						// border-radius: 50rpx 0 0rpx 50rpx;
					}
				}

				.name {
					width: 70%;
					padding-right: 40rpx;
					text-align: center;
					font-size: 30rpx;
				}
			}
		}
	}
</style>
