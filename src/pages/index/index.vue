<template>
	<view class="content">
		<!--<image v-if="imgUrl.length > 0" class="logo" :src="imgUrl"></image>-->
		<button open-type='getUserInfo'  @getuserinfo="getUserInfo">生成头像</button>
		<canvas :style="{ width: cav_width+'rpx', height: cav_height + 'rpx' }"
		canvas-id="merge-img"></canvas>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				imgUrl:'',
				cav_width:720,
				cav_height:400
			}
		},
		onLoad() {
			console.log(1)
			console.log(uni.getSystemInfoSync().windowWidth)
			console.log(2)
		},
		methods: {
			getUserInfo(){
							uni.getUserInfo({
								provider: uni.getProvider,
								success: (infoRes) => {
									console.log(infoRes)
									console.log('用户昵称为：' + infoRes.userInfo.nickName);
									this.nickName = infoRes.userInfo.nickName
									this.imgUrl = infoRes.userInfo.avatarUrl.replace('/132','/0')

									
									uni.getImageInfo({
										src:infoRes.userInfo.avatarUrl.replace('/132','/0'),
										success:(res)=>{
											let width = uni.getSystemInfoSync().windowWidth
											this.cav_width = 750
											this.cav_height = 750
											const ctx = uni.createCanvasContext('merge-img',this)
											console.log(res)
											ctx.drawImage(res.path,0,0,width,width)	
											ctx.draw(true,()=>{
												ctx.drawImage('../../static/backImg.jpg',0,0,width,width)
												ctx.draw(true,()=>{
													setTimeout(()=>{
														uni.canvasToTempFilePath({
														canvasId:'merge-img',
														/*destWidth:this.cav_width/2,
														destHeight:this.cav_height/2,*/
														width:width,
														height:width,
														success:(res)=>{
															console.log(res)
															uni.saveImageToPhotosAlbum({
																filePath:  res.tempFilePath,
																success:(res)=>{
																	console.log(res)
																},
																fail:(res)=>{
																	console.log(res)
																}
															})
														},
														fail:(res)=>{
															console.log(res)
														}
												})
													},500)
													
												})
											})
										}
									})
								},
								fail: function (infoRes) {
									console.log(infoRes)
								}
							});
						},
		}
	}
</script>

<style>

</style>
