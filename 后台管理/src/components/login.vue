<template>
	<div class="container">
		<div class="login-wrapper">
			<el-card class="login-card">
				<div class="login-header">
					<h1>宠物医院管理系统</h1>
				</div>
				<el-form :model="Form" status-icon ref="ref_Form" label-width="60px"
					class="login-form" >
					<el-form-item label="账号" >
						<el-input v-model="Form.username" placeholder="请输入用户名" prefix-icon="el-icon-user"></el-input>
					</el-form-item>
					<el-form-item label="密码" >
						<el-input type="password" v-model="Form.password" placeholder="请输入密码" prefix-icon="el-icon-lock"></el-input>
					</el-form-item>
					<el-form-item>
						<el-button type="primary" @click="submitForm(Form)" class="login-button" :loading="loading">登录</el-button>
					</el-form-item>
					
				</el-form>
				<!-- <a href="">没有账号？去注册一个</a> -->
			</el-card>
		</div>

	</div>
</template>

<script>
	export default {
		data() {
			var validateusername=(rule, value, callback) => {
					if (value === '') {
					  callback(new Error('账号不能为空'));
					} else {
					 //  if (this.ruleForm.checkPass !== '') {
						this.$refs.ref_Form.validateField('username');
					 //  }
					  callback();
					}
				};
			var validatepasssword=(rule, value, callback) => {
					if (value === '') {
					  callback(new Error('密码不能为空'));
					} else {
					 //  if (this.ruleForm.checkPass !== '') {
						this.$refs.ref_Form.validateField('password');
					 //  }
					  callback();
					}
				};
			
			return {
				Form: {
					username: '',
					password:''
				},
	
				// 接收数据传回来的数据，用户和密码
				database_username:'',
				database_password:'',
				// rule:{
				// 	username:[
				// 		{  validator: validateusername, trigger: 'blur' },
				// 		{ min: 3, max: 5, message: '长度在 3 到 5 个字符', trigger: 'blur' },
				// 		],
				// 	password:[
				// 		{ validator: validatepasssword, trigger: 'blur' },
				// 		{ min: 3, max: 5, message: '长度在 3 到 5 个字符', trigger: 'blur' }]
					
				// }
					
				
			};
		},
		methods: {
			submitForm(form) {
	
				this.getAdmin()
			
			},
			
			async getAdmin(){
				
				// console.log(this.Form.username)
				// console.log(this.Form.password)
				// 发送请求，获取数据库里的信息
				if(this.Form.username==''&&this.Form.password==''){
					this.$message.error('账号、密码不能为空');
				}else if(this.Form.username==''){
					this.$message.error('账号不能为空');
				}
				else if(this.Form.password==''){
					this.$message.error('密码不能为空');
				}
				else{
					
					let url=`manage_users/login?username=${this.Form.username}&password=${this.Form.password}`
					// console.log(url)
					
					let {data:res} = await this.$http.get(url)
					// console.log(res)
					
					if(res.status==200){
						let user=res.data[0].username
						let pasd=res.data[0].password
						window.sessionStorage.setItem("user",user)
						window.sessionStorage.setItem("pasd",pasd)
						this.$message.success('登录成功！');
						//延迟1000毫秒	//跳转到首页的界面
						setTimeout(()=> {
						  this.$router.push('index'); //路由跳转用户中心
						}, 1000);
						window.sessionStorage.setItem("token", res.tokenkey);
						
					}else{
						// console.log("登录失败！！！！！！！！")
						this.$message.error('账号或密码错误，请重新输入！');
					}
					
				}
				
				
			
				
			}
		}
	}
</script>

<style scoped>
.container {
  width: 100%;
  height: 100vh;
  background: url(../assets/8.jpg) no-repeat;
  background-size: cover;
  position: relative;
}

.login-wrapper {
  position: absolute;
  width: 450px;
  right: 15%;
  top: 50%;
  transform: translateY(-50%);
  animation: slideIn 0.8s ease-out;
}

.login-card {
  background: rgba(255, 255, 255, 0.95);
  border-radius: 8px;
  box-shadow: 0 8px 20px rgba(0, 0, 0, 0.15);
  backdrop-filter: blur(10px);
}

.login-header {
  text-align: center;
  padding: 20px 0;
}

.login-header h1 {
  font-size: 24px;
  color: #303133;
  margin: 0;
  font-weight: 600;
}

.login-form {
  padding: 20px 35px;
}

.login-button {
  width: 100%;
  height: 40px;
  font-size: 16px;
  margin-top: 10px;
  background: linear-gradient(45deg, #409EFF, #36D1DC);
  border: none;
  transition: all 0.3s ease;
}

.login-button:hover {
  transform: translateY(-2px);
  box-shadow: 0 5px 15px rgba(64, 158, 255, 0.3);
}

@keyframes slideIn {
  from {
    opacity: 0;
    transform: translate(50px, -50%);
  }
  to {
    opacity: 1;
    transform: translate(0, -50%);
  }
}

/* Element UI 输入框样式优化 */
.el-input__inner {
  height: 40px;
  line-height: 40px;
}

.el-form-item {
  margin-bottom: 25px;
}

.el-input__inner:focus {
  border-color: #409EFF;
  box-shadow: 0 0 5px rgba(64, 158, 255, 0.2);
}

/* 响应式调整 */
@media screen and (max-width: 1200px) {
  .login-wrapper {
    right: 10%;
  }
}

@media screen and (max-width: 768px) {
  .login-wrapper {
    right: 50%;
    transform: translate(50%, -50%);
  }
}
</style>
