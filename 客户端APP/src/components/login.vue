<template>
	<div class="container">

		<div class="box">
			<van-form @submit="onSubmit(form)">
				<h2>宠物医疗平台</h2>
				<van-image round width="7rem" height="7rem" :src="url" />
				<br>
				<van-cell-group inset>
					<van-field v-model="form.username" name="用户名" label="用户名" placeholder="请输入用户名"
						:rules="[{ required: true, message: '请填写用户名' }]" class="login" />
					<van-field v-model="form.password" type="password" name="密码" label="密码" placeholder="请输入密码"
						:rules="[{ required: true, message: '请填写密码' }]" class="login" />
					<br>
					
						<van-radio-group v-model="form.checked" direction="horizontal" >
							<van-radio name="1" icon-size="18px">普通用户</van-radio>
							<van-radio name="2" icon-size="18px">医生用户</van-radio>
						</van-radio-group>
					<br>
				</van-cell-group>

				<div style="margin: 16px;">
					<van-button round block type="primary" native-type="submit">登录</van-button>
				</div>
			</van-form>
			<div class="register-link">
				<a @click="register">还没有账号？立即注册</a>
			</div>
		</div>


	</div>
</template>

<script>
	import {
		Notify
	} from 'vant';
	export default {
		data() {
			return {
					form: {
						username: '',
						password: '',
						checked: ''
					},
					url: require('../assets/dog.jpg'),
					
				}
		},
		created() {

		},
		methods: {
			async onSubmit(form) {
				let userRole=form.checked
				let user = form.username
				let pasd = form.password
				if(userRole==1){
					// 普通用户登录
					let {
						data: res
					} = await this.$http.get("/app_users/appuser", {
						params: {
							username: user,
							password: pasd
						}
					})
					if (res.status == 200) {
						window.sessionStorage.setItem("id", res.data[0].id)
						window.sessionStorage.setItem("nickname", res.data[0].nickname)
						window.sessionStorage.setItem("username", res.data[0].username)
						window.sessionStorage.setItem("petname", res.data[0].petname)
						window.sessionStorage.setItem("phone", res.data[0].phone)
						window.sessionStorage.setItem('history', res.data[0].history)
						window.sessionStorage.setItem('headphoto', res.data[0].headphoto)
						this.url = res.data[0].headphoto
						this.$router.push("/home")		
					} else {		
						Notify('身份或账号或密码错误，请重新输入!');
					}
				}else if(userRole==2){
					// 医生用户登录
						let {
							data: res
						} = await this.$http.get("/doctor/doctorLogin", {
							params: {
								username: user,
								password: pasd
							}
						})
						console.log(res)
						if(res.status==200){
							window.sessionStorage.setItem("id", res.data[0].id)
							window.sessionStorage.setItem("nickname", res.data[0].nickname)
							window.sessionStorage.setItem("username", res.data[0].username)
							window.sessionStorage.setItem("petname", res.data[0].petname)
							window.sessionStorage.setItem("phone", res.data[0].phone)
							window.sessionStorage.setItem('history', res.data[0].history)
							window.sessionStorage.setItem('headphoto', res.data[0].headphoto)
							this.url = res.data[0].headphoto
							this.$router.push("/home1")
						}else{
							Notify('身份或账号或密码错误，请重新输入!');
						}	
				}else{
					Notify('请勾选身份!');
				}

			},
			register() {
				this.$router.push("/register")
			},

		}
	}
</script>

<style scoped>
	.container {
		width: 100vw;
		height: 100vh;
		position: fixed;
		top: 0;
		left: 0;
		background-image: url(../assets/background.jpg);
		background-size: cover;
		background-position: center;
		display: flex;
		justify-content: center;
		align-items: center;
	}

	.box {
		width: 400px;
		height: auto;
		min-height: 450px;
		text-align: center;
		background-color: rgba(255, 255, 255, 0.95);
		padding: 30px;
		border-radius: 15px;
		box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
	}

	.login {
		border-bottom: 1px solid #eaeaea;
		margin-bottom: 10px;
	}
	.van-radio{
		margin: 0 10px;
	}

	.van-button--primary {
		background-color: #4DBCFF;
		border: none;
		font-size: 16px;
		height: 44px;
	}

	.register-link {
		margin-top: 20px;
	}

	.register-link a {
		color: #4DBCFF;
		text-decoration: none;
		cursor: pointer;
	}

	h2 {
		color: #333;
		margin-bottom: 20px;
	}
</style>
