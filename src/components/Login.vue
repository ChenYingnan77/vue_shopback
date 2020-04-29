<template>
  <div class="login_container">
    <div class="login_box">
      <!-- 头像区域 -->
      <div class="touxiang_box">
        <img src="../assets/logo.png" alt />
      </div>
      <!-- 登陆区域 -->
      <el-form
        ref="loginFormRef"
        :model="loginForm"
        :rules="rules"
        label-width="80px"
        class="login_form"
      >
        <!-- 用户名输框 -->
        <el-form-item label="用户名:" prop="username">
          <el-input v-model="loginForm.username" prefix-icon="el-icon-user-solid"></el-input>
        </el-form-item>
        <!-- 密码输入框 -->
        <el-form-item label="密码:" prop="password">
          <el-input type="password" v-model="loginForm.password" prefix-icon="el-icon-lock"></el-input>
        </el-form-item>
        <!-- 登陆和重置按钮 -->
        <el-form-item class="btns">
          <el-button type="primary" @click="login">登陆</el-button>
          <el-button @click="resetloginForm">重置</el-button>
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      //登陆表单的数据绑定
      loginForm: {
        username: "admin",
        password: "123456"
      },
      //表单验证规则
      rules: {
        username: [
          { required: true, message: "请输入用户名", trigger: "blur" },
          { min: 3, max: 5, message: "长度在 3 到 5 个字符", trigger: "blur" }
        ],
        password: [
          { required: true, message: "请输入密码", trigger: "blur" },
          { min: 6, max: 10, message: "长度在 6 到 10 个字符", trigger: "blur" }
        ]
      }
    };
  },
  methods: {
      //重置登录信息
    resetloginForm() {
      this.$refs.loginFormRef.resetFields();
    },
    //点击登陆发送请求
    login() {
      this.$refs.loginFormRef.validate(async valid => {
        if (!valid) {
          return;
        }
        const { data: res } = await this.$http.post("login", this.loginForm);
        console.log(res);
        if (res.meta.status == 200) {
          this.$message.success("登陆成功");
          window.sessionStorage.setItem("token", res.data.token);
          this.$router.push('/home');
        } else {
          this.$message.error("登陆失败");
        }
      });
    }
  }
};
</script>

<style scoped>

.login_container {
  position: relative;
  background-color: #2b4b6b;
  height: 100%;
}

.login_box {
  position: absolute;
  width: 450px;
  height: 300px;
  background-color: #fff;
  border-radius: 3px;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
}

.touxiang_box {
  position: absolute;
  width: 130px;
  height: 130px;
  left: 160px;
  top: -65px;
  border: 1px solid #eee;
  border-radius: 50%;
  padding: 10px;
  box-shadow: 0 0 10px #ddd;
  background-color: #fff;
}

.touxiang_box img {
  width: 100%;
  height: 100%;
  border-radius: 50%;
}

.login_form {
  position: absolute;
  bottom: 0;
  width: 100%;
  padding: 0 20px 0 0;
  box-sizing: border-box;
}

.btns {
  display: flex;
  justify-content: flex-end;
}
</style>