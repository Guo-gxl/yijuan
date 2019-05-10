<template>
 <div class="reg-container">
  <div> 
      <header id="header" class="mui-bar mui-bar-nav">
			<a @click="gobacktologin" class="mui-action-back mui-icon mui-icon-left-nav mui-pull-left"></a>
			<h1 class="mui-title">注册账号</h1>
		</header>        
</div>
    <el-row type="flex" justify="center">
        <el-form ::model="user" ref="loginForm" :model="user" :rules="rules" class="mui-input-group demo">
            <el-form-item class="mui-input-row" prop="name">
                  <label>账号</label>
                <el-input v-model="user.name" type="text" required="required" placeholder="请输入账号"></el-input>
            </el-form-item> 
            
            <el-form-item class="mui-input-row" prop="pass">
                 <label>密码</label>
                <el-input v-model="user.pass" type="password" required="required" placeholder="请输入密码"></el-input>
            </el-form-item>
             <el-form-item class="mui-input-row" prop="checkPass">
                 <label></label>
                <el-input v-model="user.checkPass" type="password" required="required" placeholder="请再次输入密码"></el-input>
            </el-form-item>
            <el-form-item class="mui-input-row" prop="sex">
                <label>性别</label>
                <!-- <input v-model="user.sex" type="text" required="required" class="mui-input-clear mui-input" placeholder="请输入账号"> -->
        <el-select v-model="user.sex" class="mui-input-clear mui-input" placeholder="未选择">
            <el-option value="男">男</el-option>
            <el-option value="女">女</el-option>
        </el-select>
            </el-form-item>
            <el-form-item class="mui-input-row" prop="nickName">
                  <label>昵称</label>
                <el-input v-model="user.nickName" type="text" required="required" class="mui-input-clear mui-input" placeholder="请输入昵称"></el-input>
            </el-form-item>
             <el-form-item class="mui-input-row" prop="phone">
                  <label>手机号</label>
                <el-input v-model="user.phone" type="text" required="required" class="mui-input-clear mui-input" placeholder="请输入手机号"></el-input>
            </el-form-item>  
             <el-form-item class="mui-input-row" prop="email">
                <label>邮箱</label>
                <el-input v-model="user.email" type="email" required="required" class="mui-input-clear mui-input" placeholder="请输入邮箱"></el-input>
            </el-form-item>
            <div class="mui-content-padded">
				<button type="button" @click="register" class="mui-btn mui-btn-block mui-btn-primary">注册</button>
				<div class="link-area"><router-link to="/login">登录</router-link>
				</div>
			</div>
        </el-form>
    </el-row>
 </div>
</template>

<script>
import { Toast } from "mint-ui";
    export default {
        created:function () {
            this.$emit('public_header', false);
            this.$emit('public_footer', false);
            this.$emit('public_headern', false);
        },
        methods: {
            gobacktologin(){
             this.$router.replace('/login')
            },
    register() {
               
    this.$refs.loginForm.validate((valid) => {
        if (valid) {
            if(this.user.sex!='0'){
            this.$ajax.post('http://47.103.14.235:27499/users/validate', this.user).then((res) => {
                if (res.data) {
                    this.$store.dispatch('login', res.data).then(() => {
                        // this.$notify({
                        //     type: 'success',
                        //     message: '注册成功，请登录',
                        //     duration: 1000
                        // })
                        Toast('注册成功，请登录');
                        this.$router.replace('login')
                    })
                } else {
                    // this.$message({
                    //     type: 'error',
                    //     message: '请输入注册信息',
                    //     showClose: true
                    // })
                     Toast('请将信息输入完整再提交');
                }
            }).catch((err) => {
                this.$message({
                    type: 'error',
                    message: '网络错误，请重试',
                    showClose: true
                })
            })
        }
        else {
            Toast('请将信息输入完整再提交');
        }
        
        }
    })
}
        },
        data () {

            var validatePass = (rule, value, callback) => {
        if (value === '') {
          callback(new Error('请输入密码'));
        } else {
          if (this.user.checkPass !== '') {
            this.$refs.loginForm.validateField('checkPass');
          }
          callback();
        }
      };
      var validatePass2 = (rule, value, callback) => {
        if (value === '') {
          callback(new Error('请再次输入密码'));
        } else if (value !== this.user.pass) {
          callback(new Error('两次输入密码不一致!'));
        } else {
          callback();
        }
      };
            return {
                user: {
                },
                rules: {
                    name: [
            { required: true, message: '请输入帐号', trigger: 'blur' },
            { min: 8, max: 16, message: '长度在 8 到 16 个字符', trigger: 'blur' }
          ],
                    pass: [
            { validator: validatePass, trigger: 'blur' }
          ],
          checkPass: [
            { validator: validatePass2, trigger: 'blur' }
          ],
          nickName: [
            { required: true, message: '请输入用户昵称', trigger: 'blur' },
            { min: 3, max: 16, message: '长度在 3 到 16 个字符', trigger: 'blur' }
          ],
          phone: [
            { required: true, message: '请输入手机号', trigger: 'blur' },
            { min: 11, max: 11, message: '请正确输入手机号', trigger: 'blur' }
          ],
          email: [
            { type: 'email', required: true, message: '请输入邮箱号', trigger: 'blur' }
          ],
           sex: [
            { required: true, message: '请选择活动区域', trigger: 'change' }
          ],
                },
            }
        }
    }
</script>
<style lang="scss" scoped>
.reg-container{
    margin-top: 40px;
}
.mui-input-group {
				width: 100%;
            }
.area {
				margin: 20px auto 0px auto;
			}
			
			.mui-input-group {
				margin-top: 10px;
			}
			
			.mui-input-group:first-child {
				margin-top: 20px;
			}
			
			.mui-input-group label {
				width: 22%;
			}
			.mui-input-row{
                height: 60px;
            }
			.mui-input-row label~input,
			.mui-input-row label~select,
			.mui-input-row label~textarea {
				width: 78%;
			}
			
			.mui-checkbox input[type=checkbox],
			.mui-radio input[type=radio] {
				top: 6px;
			}
			
			.mui-content-padded {
				margin-top: 25px;
			}
			
			.mui-btn {
                padding: 10px;
                margin:0 auto;
			}
			
			.link-area {
				display: block;
				margin-top: 25px;
				text-align: center;
			}
			
			.spliter {
				color: #bbb;
				padding: 0px 8px;
			}
			
			.oauth-area {
				position: absolute;
				bottom: 20px;
				left: 0px;
				text-align: center;
				width: 100%;
				padding: 0px;
				margin: 0px;
			}
			
			.oauth-area .oauth-btn {
				display: inline-block;
				width: 50px;
				height: 50px;
				background-size: 30px 30px;
				background-position: center center;
				background-repeat: no-repeat;
				margin: 0px 20px;
				/*-webkit-filter: grayscale(100%); */
				border: solid 1px #ddd;
				border-radius: 25px;
			}
			
			.oauth-area .oauth-btn:active {
				border: solid 1px #aaa;
			}
			
			.oauth-area .oauth-btn.disabled {
				background-color: #ddd;
            }
           .el-input{
               width: 75%
           }
           .mui-title {
    font-size: 19px;
    }
    .mui-btn-primary{
    color: #fff;
        background-color: salmon;
}
.el-form-item{
    margin-bottom: 0px !important;
}
.demo /deep/ .el-form-item__error{
    left: 25% !important; 
       
}
label{
    font-size: 16px;
}
.demo /deep/ .el-input__inner{
    font-size: 16px;
    height: 40px;
}
</style>