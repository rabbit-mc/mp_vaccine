<template>
	<view class="page">
		<view v-show="list.length>0" class="box" v-for="(item,index) in list" :key="index" @click="jz(item.id)">
			<view>接种人姓名：{{item.us_name}}</view>
			<view>疫苗名称：{{item.va_name}}</view>
			<view>接种日期：{{item.appoint_date}}</view>
			<view>接种时间：{{datefor(item.time_slot)}}</view>
		</view>
		<view v-show='list.length<=0'>
			<u-empty text="暂无人员接种" mode="search"></u-empty>
		</view>
	</view>
</template>

<script>
	export default {
		name: "allList",
		data(){
			return {
				list: [],
			}
		},
		onLoad(){
			this.init();
		},
		methods: {
			init(){
				// console.log(uni.getStorageSync("userInfo").worker.inoculate_site_id)
				this.$u.get(`/worker/allList?id=${uni.getStorageSync("userInfo").worker.inoculate_site_id}`).then(result=>{
					this.list = result
				})
			},
			jz(value){
				uni.navigateTo({
					url: `./jieZhong?id=${value}`
				})
			},
			datefor(value){
				switch(value){
					case 0:
						return '9点 - 12点'
						break
					case 1:
						return '14点 - 18点' 
						break
					case 2:
						return '19点 - 23点'
						break
				}
			}
		}
	}
</script>

<style scoped>
.box {
	margin-bottom: 10px;
}
</style>