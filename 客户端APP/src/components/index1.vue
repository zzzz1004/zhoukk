<template>
	<div>
		<div class="header">
			<van-image width="100%" height="250px" :src="url" />
		</div>
		<div class="baiban">
			<br>
			<div class="content">
				<van-grid :gutter="10" :column-num="3">
					<van-grid-item v-for="value in list3" @click="btn(value.path)" :key="value.id" :icon="value.photo" :text="value.text" />
				</van-grid>
			</div>
			
			
			<div class="content">
				<van-grid :gutter="10" :column-num="3">
					<van-grid-item v-for="value in list2" @click="btn(value.path)" :key="value.id" :icon="value.photo" :text="value.text" />
				</van-grid>
			</div>
			<hr>

			<div>
				<div><img src="../assets/10.jpg" width="100%" height="300px" alt=""></div>
			</div>	

		</div>
	</div>
</template>

<script>
	export default {
		data() {
			return {
				url: require('../assets/1.png'),
				list3:[
					{	id:'7',
						text: '在线请假',
						photo: 'comment-o',
						path:'/online_qingjia'
					},
					{	id:'8',
						text: '在线打卡',
						photo: 'comment-o',
						path:'/online_daka'
					},
					{	id:'9',
						text: '我的排班',
						photo: 'comment-o',
						path:'/myZhiban'
					},
				
				],
				list2:[
					{	id:'4',
						text: '医院公告',
						photo: 'comment-o',
						path:'/gongGao'
					},
					{	
						id:'5',
						text: '环境设备',
						photo: 'comment-o',
						path:'/environment'
					},	
					{
						id:'6',
						text: '常见疾病治疗',
						photo: 'comment-o',
						path:'/yi_liao'
					},
				]
			}
		},
		methods:{
			btn(path){
				console.log(path)
				this.$router.push(path)
			}
		}
	}
</script>

<style scoped="scoped">

    .baiban {
		background-color: #F7F7F7;
		height: 460px;
		border-radius: 25px 25px 0px 0px;
		position: relative;
		top: 0px;
	}
</style>
