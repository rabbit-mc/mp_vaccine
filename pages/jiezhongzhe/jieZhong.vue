<template>
	<view class="page">
		<view v-show="list.length>0">
			<view class="box" v-for="item in list" :key="item.id">
				<view class="info">
					<view>接种地点：{{item.in_name}}</view>
					<view>疫苗名称：{{item.va_name}}</view>
					<view>预约时间：{{item.appoint_date}}</view>
				</view>
				<view class='btn'>
					<u-button size="medium" @click="cancel(item.id)">取消预约</u-button>
				</view>
			</view>
			<!-- <u-image width="650rpx" height="650rpx" :src="qrCodePath" @click="twice()"></u-image>
			<view>当前状态是：<text style="color: red;">{{status}}</text>(点击二维码刷新状态)</view>
			<view style="margin-top: 20rpx;" v-if="show2">
				<button type="warn" @click="cancel">取消预约</button>
			</view> -->
			<!-- <view v-else>
				<text>您已在接种点完成签到，请按流程继续完成后续接种工作！</text>
			</view> -->
		</view>
		<view v-show='list.length==0'>
			<u-empty text="暂无预约" mode="search"></u-empty>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				qrCodePath:"",
				show:false,
				show2:false,
				status:"",
				list:[],
			}
		},
		onLoad(option) {
			// this.planId = option.planId;
			// this.form.planId = option.planId;
			// this.form.timeSlot = option.timeSlot;
			// this.form.appointDate = option.appointDate;
			this.init();
		},
		methods: {
			twice() {
				// this.qrCodePath = "";
				this.show = false;
				this.show2 = false;
				this.status = "";
				this.init();
			},
			init(){
				this.$u.get(`/user/findAppoint/?userId=${uni.getStorageSync("userInfo").user.id}`).then(res=>{
					if(res.code == 0){
						this.list = res.data
						console.log(this.list)
						// if(res.data.length == 1) {
						// 	// this.qrCodePath = "http://localhost:8080/qrCodeImage/" + res.data[0].qrCodeUrl;
						// 	if (res.data[0].status == 0) {
						// 		this.show2 = true;
						// 		this.status = "待签到";
						// 	} else if (res.data[0].status == 1) {
						// 		this.status = "待预检";
						// 	} else if (res.data[0].status == 2) {
						// 		this.status = "待接种";
						// 	} else if (res.data[0].status == 3) {
						// 		this.status = "留观中";
						// 	}
						// 	this.show = true;
						// }
						// console.log(this.qrCodePath);
					}
				})
			},
			cancel(value) {
				uni.showModal({
				    title: '取消预约',
				    content: '您确定要取消预约吗？',
				    success: res => {
				        if (res.confirm) {
							console.log(value)
				            this.$u.put(`/user/cancelAppoint?id=${value}`).then(res=>{
								if(res.code == 0) {
									uni.showToast({
									title: '取消预约成功',
									duration: 1000
									});
									this.qrCodePath = "";
									this.show = false;
									this.show2 = false;
								} else {
									uni.showToast({
									title: '服务端错误',
									duration: 1000
									});
								}
							}).finally(()=>{
								this.init()
							})
				        } 
				    }
				});
			}
		}
	}
</script>


<style scoped>
.box {
	display: flex;
	margin-bottom: 10px;
	position: relative;
	align-items: center;
}
.info {
	display: flex;
	flex-direction: column;
}
.btn {
	position: absolute;
	right: 0;
}
</style>
