<template>
  <body id="poster">
  <el-form :model="loginForm" :rules="rules" ref="loginFormRef" class="login-container" label-position="left"
           label-width="0px" v-loading="loading">
    <h3 class="login_title">Fox综合人事管理系统登录</h3>
    <el-form-item prop="username" >
      <el-input type="text" v-model="loginForm.username" auto-complete="off" placeholder="请输入用户名"></el-input>
    </el-form-item>
    <el-form-item  prop="password">
      <el-input type="password" v-model="loginForm.password" auto-complete="off" placeholder="请输入密码"></el-input>
    </el-form-item>
    <el-checkbox class="login_remember" v-model="checked" label-position="left">记住密码</el-checkbox>
    <el-form-item style="width: 100%">
<!--      <el-button type="primary" @click.native.prevent="submitClick" style="width: 100%">登录</el-button>-->
      <el-button type="primary" style="width:48%;" @click.native.prevent="resetLoginForm">重 置</el-button>
      <el-button type="primary" style="width:48%;" @click.native.prevent="submitClick" :loading="logining">登 录</el-button>
    </el-form-item>
  </el-form>
  </body>
</template>
<script>
  export default{
    data(){
      return {
        rules: {
          username: [{required: true, message: '请输入用户名', trigger: 'blur'}],
          password: [{required: true, message: '请输入密码', trigger: 'blur'}]
        },
        checked: true,
        loginForm: {
          username: '',
          password: ''
        },
        loading: false
      }
    },
    methods: {
      submitClick: function () {
        var _this = this;
        this.loading = true;
        this.postRequest('/login', {
          username: this.loginForm.username,
          password: this.loginForm.password
        }).then(resp=> {
          _this.loading = false;
          if (resp && resp.status == 200) {
            var data = resp.data;
            _this.$store.commit('login', data.msg);
            var path = _this.$route.query.redirect;
            _this.$router.replace({path: path == '/' || path == undefined ? '/home' : path});
          }
        });
      },
      resetLoginForm() {
        this.$refs['loginFormRef'].resetFields();
      }
    }
  }
</script>
<style>
  #poster {
    background:url("../assets/background.jpg") no-repeat;
    background-position: center;
    height: 100%;
    width: 100%;
    background-size: cover;
    position: fixed;
  }
  body{
    margin: 0px;
  }
  .login-container {
    border-radius: 15px;
    background-clip: padding-box;
    margin: 180px auto;
    width: 350px;
    padding: 35px 35px 15px 35px;
    background: #fff;
    border: 1px solid #eaeaea;
    box-shadow: 0 0 25px #cac6c6;
  }

  .login_title {
    margin: 0px auto 40px auto;
    text-align: center;
    color: #505458;
  }

  .login_remember {
    margin: 0px 0px 35px 0px;
    text-align: left;
  }
</style>
