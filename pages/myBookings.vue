<template >
	<div class="MyBookings" >
		<HeaderCommon :logIn="logIn" :isAnonymity="1"></HeaderCommon>
		<div class="MyBookingslist" >
			<MenuTab :menu="menu=0" :logIn="logIn"></MenuTab>
			<div class="bookingsCont">
				<ul v-if="activityList.length>0||bookList.length>0">
					<li :key="index" v-for="(item,index) in activityList" v-if="activityList.length>0">
						<h3>{{item.activityInfo.title}}</h3>
						<div class="info clearfix">
							<div class="activitiesImg">
								<img :src="item.activityInfo.coverPhotoUrl">
							</div>
							<div class="activitiesText">
								<div class="dataId">
									<span>Booking ID: {{item.orderId}}</span>
									<span>Booking Time: {{item.createTime}} (EST)</span>
								</div>
								<p><b>Service Type:</b>{{item.activityInfo.category}} </p>
								<p>
									<b>Participants:</b> 
									<span v-if="item.adultNum==1">1 Adult</span>
									<span v-else>{{item.adultNum}} Adults</span>
									<span v-if="item.childrenNum!=0">,</span>
									<span v-if="item.childrenNum!=0">
										<span v-if="item.childrenNum==1">1 Child</span>
										<span v-else>{{item.childrenNum}} Children</span>
									</span>
									<span v-if="item.infantNum!=0">,</span>
									<span v-if="item.infantNum!=0">
										<span v-if="item.infantNum==1">1 Baby</span>
										<span v-else>{{item.infantNum}} Babies</span>
									</span>
									
									
								</p>
								
								<p><b>Date and Time: </b>{{item.startDate}}&nbsp;&nbsp;&nbsp;&nbsp;{{item.startTime}}</p>
								<div class="tag">
									<span  @click="goGuideDatil(0,index)">Book Again</span>
									<!--<span @click="downLoad(index)" v-if="item.status!='PAYMENT_PENDING'&&item.status!='REFUNDING'&&item.status!='REFUNDED'&&item.status!='CANCELED'">Download Contract</span>-->

									<span v-if="item.status=='PAYMENT_PENDING'" @click="goPay(0,index)">To Pay</span>
									<span v-if="(item.status=='PAYMENT_SUCCESS'||item.status=='CONFIRM_WAITING'||item.status=='BOOKING_SUCCESS')&&item.allowRefund==1" @click="Refund(0,index)">Refund</span>
								</div>
							</div>
							<div class="bookagain">
								<b class="font-color" v-if="item.status=='PAYMENT_PENDING'">Payment pending</b>
								<b class="font-color" v-if="item.status=='PAYMENT_SUCCESS'||item.status=='CONFIRM_WAITING'">To be confirmed</b>
								<b class="font-color1" v-if="item.status=='BOOKING_SUCCESS'">Confirmed</b>
								<b class="font-color1" v-if="item.status=='COMPLETED'">Tour completed</b>
								<b class="font-color" v-if="item.status=='REFUNDING'">Refund in progress</b>
								<b class="font-color1" v-if="item.status=='REFUNDED'">Refund completed</b>
								<b class="font-color1" v-if="item.status=='CANCELED'">Canceled</b>
								<p>${{item.amount}}</p>
								<!--<button class="btnlinner margin">VIEW DETAILS</button>-->

							</div>
						</div>
					</li>
					<li :key="index" v-for="(item,index) in bookList" v-if="bookList.length>0">
						<h3 v-if="item.guideInfo.enName">{{item.guideInfo.enName}}</h3>
						<div class="info clearfix">
							<div class="activitiesImg">
								<img :src="item.guideInfo.headPortraitUrl">
							</div>
							<div class="activitiesText">
								<div class="dataId">
									<span>Booking ID: {{item.orderId}}</span>
									<span>Booking Time: {{item.createTime}} (EST)</span>
								</div>
								<p v-if="item.serviceType=='HIKING'"><b>Service Type:</b> Tour guide</p>
								<p v-else><b>Service Type:</b>Tour guide with a car</p>
								<p><b>Participants:</b> {{item.participants}}</p>
								<p v-if="item.fullDates"><b>Full day date:</b>{{item.fullDates.join(",")}}</p>
								<p v-if="item.halfDates"><b>Half day date:</b>{{item.halfDates.join(",")}}</p>
								<div class="tag">
									<span  @click="goGuideDatil(1,index)">Book Again</span>
									<span @click="downLoad(index)" v-if="item.status!='PAYMENT_PENDING'&&item.status!='REFUNDING'&&item.status!='REFUNDED'&&item.status!='CANCELED'">Download Contract</span>

									<span v-if="item.status=='PAYMENT_PENDING'" @click="goPay(1,index)">To Pay</span>
									<span v-if="(item.status=='PAYMENT_SUCCESS'||item.status=='CONFIRM_WAITING'||item.status=='BOOKING_SUCCESS')&&item.allowRefund==1" @click="Refund(1,index)">Refund</span>
								</div>
							</div>
							<div class="bookagain">
								<b class="font-color" v-if="item.status=='PAYMENT_PENDING'">Payment pending</b>
								<b class="font-color" v-if="item.status=='PAYMENT_SUCCESS'||item.status=='CONFIRM_WAITING'">To be confirmed</b>
								<b class="font-color1" v-if="item.status=='BOOKING_SUCCESS'">Confirmed</b>
								<b class="font-color1" v-if="item.status=='COMPLETED'">Tour completed</b>
								<b class="font-color" v-if="item.status=='REFUNDING'">Refund in progress</b>
								<b class="font-color1" v-if="item.status=='REFUNDED'">Refund completed</b>
								<b class="font-color1" v-if="item.status=='CANCELED'">Canceled</b>
								<p>${{item.amount}}</p>
								<!--<button class="btnlinner margin">VIEW DETAILS</button>-->

							</div>
						</div>
					</li>
				</ul>
				 <div class="empty" v-else>
					<span>
						<svg class="icon" aria-hidden="true">
						    <use xlink:href="#icon-lvyou"></use>
						</svg>
					</span>
					<p>You have no booking here.</p>
				</div>
			</div>
			
		</div>
		<div id="downNode"></div>
		<FooterCommon v-if="logIn==1"></FooterCommon>
		<Refund :businessType="businessType" :isShowAlertTitle="isShowAlertTitle" :orderId="orderId" :alertTitle="alertTitle" :alertMessage="alertMessage" v-on:setIsShowAlertFn="getIsShowAlertFn" v-on:setIsShowConfirmFn="getShowConfirmFn"></Refund>
	</div>
	
</template>

<script>
	import{GetQueryString} from '~/assets/js/plugin/utils';
	import HeaderCommon from '~/components/HeaderCommon/HeaderCommon';
    import FooterCommon from '~/components/FooterCommon/FooterCommon';
    
	import Refund from '~/components/pageComponents/myBookings/Refund';
    import MenuTab from "~/components/MenuTab";
    
	export default {

		name: 'MyBookings',

		async asyncData({ route, store, error, apiBasePath, redirect }) {
			let menu = route.query.menu || 0;
			let flag = route.query.flag || 1;
			let data = {
				activityList:'',
				logIn: '',
				bookList: '',
				isShowAlertTitle: '',
				alertMessage: '',
				alertTitle: '',
				orderId: '',
				url: '',
				businessType:'',
				apiBasePath
			}
			return data;
		},

		components: {
			MenuTab,
			HeaderCommon,
			FooterCommon,
			Refund
		},

		methods: {
			getShowConfirmFn(val) {
				this.isShowAlertTitle = val;
			},
			getIsShowAlertFn(val) {
				this.isShowAlertTitle = val;
			},
			goPay(id,index) {
				if(id==0){
					window.location.href = "payForActivity.html?objectId=" + this.activityList[index].orderId
				}else{
					window.location.href = "payNow.html?objectId=" + this.bookList[index].orderId
				}
				
			},
			goGuideDatil(id,index) {
				if(id==0){
					window.location.href = "/activity/details/" + this.activityList[index].activityId
				}else{
					window.location.href = "guide/detail/" + this.bookList[index].guideId
				}
				
			},
			downLoad(index) {
				var node = document.getElementById("downNode");
				var html = '<form method="POST" target="_blank" action="'+ this.apiBasePath + 'order/contract/download">' +
					'<input name="orderId" type="hidden"  value="' + this.bookList[index].orderId + '">' +
					'</form>';
				node.innerHTML = html;
				var fd = node.querySelector('form');
				fd.submit();
			},
			Refund(id,index) {
				let that = this
				if(id==0){
					that.orderId=that.activityList[index].orderId
					that.businessType="ACTIVITY"
					
				}else{
					that.orderId = that.bookList[index].orderId
					that.businessType="GUIDE_SERVICE"
				}
				
				//oderId:110134390
				that.isShowAlertTitle = true
				that.alertMessage = " You are currently requesting a refund. Please select confirm to proceed with your order refund request, otherwise press cancel to return."

			}
		},

		mounted: function() {
			let that = this
			that.logIn = window.localStorage.getItem("logstate")
			var obj
			if(localStorage.getItem("userid")!=null){
				 obj = {
					"userId": localStorage.getItem("userid")
					//userId:10024
				}
			}else if(GetQueryString("orderId")){
				 obj={
					orderId:GetQueryString("orderId"),
					emailAddress:GetQueryString("emailAddress")
				}
			}else{
				 obj=JSON.parse(localStorage.getItem("obj"))
			}
			
			that.axios.post(this.apiBasePath + "activity/order/list", JSON.stringify(obj), {
				headers: {
					'Content-Type': 'application/json; charset=UTF-8'
				}
			}).then(function(response) {
				console.log(response.data)
				that.activityList=response.data
			}, function(response) {})
			that.axios.post(this.apiBasePath + "order/list", JSON.stringify(obj), {
				headers: {
					'Content-Type': 'application/json; charset=UTF-8'
				}
			}).then(function(response) {
				console.log(response.data)
				that.bookList = response.data
			}, function(response) {})
		}
	}
</script>
<style lang="scss">
	@import '~assets/scss/_main.scss';
	@import '~/assets/font/iconfont.css';
	#header {
		box-shadow: 0px 2px 6px 0px rgba(53, 58, 63, 0.1);
	}
</style>
<style lang="scss" scoped>
	@import "~assets/scss/base/_setting.scss";
	.icon {
		width: 196px;
		height: 196px;
		vertical-align: -0.15em;
		fill: currentColor;
		overflow: hidden;
		
	}
	
	.MyBookings {
		.MyBookingslist {
			width: 1170px;
			margin: 0 auto;
			position: relative;
			.bookingsCont {
				margin-left: 250px;
				padding: 60px 0 100px 60px;
				box-shadow: -1px 0px 0px 0px rgba(53, 58, 63, 0.1);
				ul {
					li {
						padding: 30px 0;
						border-bottom: 1px solid #dde0e0;
						&:first-child {
							border-top: none;
							padding-top: 0;
						}
						h3 {
							font-size: 24px;
							font-weight: bold;
							margin-bottom: 22px;
						}
						.info {
							.activitiesImg {
								float: left;
								width: 230px;
								height: 186px;
								img {
									width: 100%;
									height: 100%;
								}
							}
							.activitiesText {
								float: left;
								margin-left: 24px;
								width: 460px;
								.dataId {
									font-size: 14px;
									margin-bottom: 10px;
									span {
										margin-right: 28px;
										&:last-child{
											margin-right: 0;
										}
									}
								}
								p {
									margin-bottom: 8px;
									font-size: 18px;
									b {
										font-size: 18px;
										margin-right: 5px;
									}
								}
								.tag {
									font-size: 16px;
									margin-top: 15px;
									span {
										color: #353a3f;
										position: relative;
										padding-right: 16px;
										text-decoration: underline;
										cursor: pointer;
										&:not(:last-child) {
											margin-right: 16px;
											&:after {
												content: '';
												position: absolute;
												height: 10px;
												width: 2px;
												background: #878e95;
												top: 7px;
												right: 0;
											}
										}
									}
								}
							}
							.bookagain {
								float: right;
								b {
									font-size: 16px;
									margin-top: 46px;
									&.font-color {
										color: #d87b65;
									}
									&.font-color1 {
										color: #878e95;
									}
								}
								p {
									margin-top: 40px;
									font-size: 22px;
									font-weight: bold;
									text-align: right;
								}
								.margin {
									margin-top: 68px;
								}
							}
						}
					}
				}
			}
		}
		.empty {
			text-align: center;
			margin: 90px 0 184px;
			p{
				font-size: 18px;
				font-weight: bold;
				margin-top: 34px;
			}
		}
	}
</style>