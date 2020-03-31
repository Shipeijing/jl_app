<template>
	<view class="content">
		<el-amap class="amap-box" vid="map" 
		        :zoom="zoom"
				mapStyle="fresh"
		        :center="center">
		</el-amap>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				myMapSocket:null,
				zoom:16,
				center:[121.406051,31.179695],
			}
		},
		onLoad() {
		const _this=this
       // setInterval(this.getLocation(),2000)
	     this.myMap=uni.connectSocket({
		  url: 'ws://localhost:80/map',
		  success:(res)=>{
			  console.log("连接服务器成功")
		  }
		});
		this.myMap.onOpen(function () {
		  console.log('连接已打开');
		 // _this.sendData('我是小明')
		});
		this.myMap.onMessage(function (res) {
		  console.log('收到服务器内容：' + res.data);
		});
		this.myMap.onClose(function () {
		  console.log('连接已关闭');
		});
		this.myMap.onError(function(){
			 console.log('连接出错');
		})
		},
		methods: {
		sendData(data){
			this.myMap.send({
				data: data,
				success:(res)=>{
					console.log('发送消息：'+data);
				},fail:(res)=>{
					console.log('发送失败：'+res);
				}
			})
		},
         getLocation(){
			 //获取位置信息
			 uni.getLocation({
			     type: 'wgs84',
			     success: function (res) {
					 this.center=[121.406051,31.179695]
			     },fail:(res)=>{
					 this.center=[121.406051,31.179695]
				 }
				 
			 });
		 }
		}
	}
</script>

<style>
	.content {
		height:calc(100vh - 94px);
	}

	.index-map {
		width:100%;
		height:calc(100vh - 94px);
	}
</style>
