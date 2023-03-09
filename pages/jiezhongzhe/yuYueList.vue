<template>
	<view>
		<view class="uni-list-cell" hover-class="uni-list-cell-hover" v-for="(value, key) in list" :key="key" style="margin-top: 10rpx;">
			<view class="uni-media-list" style="border-bottom: 2rpx solid #eeeeee;">
				<view>
					<text style="line-height: 200rpx; font-size: 30rpx;">疫苗名称：{{value.va_ame}}</text>
				</view>
				<view class="uni-media-list-body">
					<view class="uni-media-list-text-top">
							<text>{{value.create_time}}</text>
					</view>
					<view class="uni-media-list-text-bottom">
						<text>接种点：{{value.in_name}}</text>
					</view>
					<view class="uni-media-list-text-bottom">
						<text>预约时间段:{{value.timeslot}}</text>
					</view>
					<view class="uni-media-list-text-bottom">
						<text>预约日期：{{value.appoint_date}}</text>
					</view>
					<view class="uni-media-list-text-bottom">
						<text style="color: red;">当前状态：{{value.flag}}</text>
					</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				list:[]
			}
		},
		onLoad() {
			this.init();
		},
		methods: {
			init(){
				this.$u.get(`/user/findAppointHistories?userId=${uni.getStorageSync("userInfo").user.id}`).then(res=>{
					if(res.code === 0){
						res.data.forEach(item=>{
							switch(item.time_slot){
							  case 0:
								item.timeslot = '9点 - 12点'
								break
							  case 1:
								item.timeslot = '14点 - 18点' 
								break
							  case 2:
								item.timeslot = '19点 - 23点'
								break
							}
							switch(item.status){
							  case 1:
								item.flag = '预约成功'
								break
							  case 2:
							  item.flag = '接种完成'
								break
							  case 3:
								item.flag = '预约过期'
								break
							  case 4:
								item.flag = '预约取消'
								break
							  default:
								item.flag = '预约异常'
							}
							this.list.push(item)
						  })
					}
				})
			}
		}
	}
</script>

<style lang="scss">
.uni-media-list{
        display: flex;
        flex-direction: row;
        margin-left: 32rpx;
        margin-right: 32rpx;
        margin-top: 20rpx;
        
        .uni-media-list-logo {
            width: 180rpx;
            height: 180rpx;
        }
        
        
        .uni-media-list-body {
            flex-direction: row;
            height: auto;
            margin-left: 32rpx;
            margin-right: 1rpx;
            justify-content: space-around;
        }
         
        .uni-media-list-text-top {
            height: 40rpx;
            font-size: 30rpx;
            overflow: hidden;
        }
         
        .uni-media-list-text-bottom {
            display: flex;
            flex-direction: row;
            margin-top: 10rpx;
            margin-right: 20rpx;
            font-size: 27rpx;
            color: #999999;
        }
    }
</style>
