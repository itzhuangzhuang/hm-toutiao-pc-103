<template>
  <div class="container-login">
    <el-card>
      <img class="logo" src="../../assets/logo_index.png" alt />
      <!-- 绘制表单 -->
      <el-form ref="loginForm" :model="loginForm" :rules="loginRules" status-icon>
        <el-form-item prop="mobile">
          <el-input v-model="loginForm.mobile" placeholder="请输入手机号"></el-input>
        </el-form-item>
        <el-form-item prop="code">
          <el-input
            style="width:240px;margin-right:8px"
            v-model="loginForm.code"
            placeholder="请输入验证码"
          ></el-input>
          <el-button>发送验证码</el-button>
        </el-form-item>
        <el-form-item>
          <el-checkbox :value="true">我已阅读并同意用户协议和隐私条款</el-checkbox>
        </el-form-item>
        <el-form-item>
          <el-button @click="login()" type="primary" style="width:100%">登 录</el-button>
        </el-form-item>
      </el-form>
    </el-card>
  </div>
</template>

<script>
export default {
  name: "login-page",
  data() {
    // 声明自定义校验函数
    const checkMobile = (rule, value, callback) => {
      // 进行校验，校验逻辑：1开头 + 第二位数字（3-9） + 9位数字结尾
      if (!/^1[3-9]\d{9}$/.test(value))
        return callback(new Error("手机号不正确"));
      callback();
    };
    return {
      loginForm: {
        mobile: "",
        code: ""
      },
      // 校验规则对象
      loginRules: {
        // 具体的校验规则
        mobile: [
          { required: true, message: "请输入手机号", trigger: "blur" },
          // 自定义校验
          { validator: checkMobile, trigger: "blur" }
        ],
        code: [
          { required: true, message: "请输入验证码", trigger: "blur" },
          { len: 6, message: "请输入6个字符", trigger: "blur" }
        ]
      }
    };
  },
  methods: {
    // 登录
    login() {
      // 1. 对整体表单进行校验
      this.$refs.loginForm.validate(valid => {
        if (valid) {
          // 2. 校验成功后进行登录
          this.$http
            .post("/authorizations", this.loginForm)
            .then(() => {
              // 成功
              this.$router.push("/");
            })
            .catch(() => {
              // 提示
              this.$message.error("手机号或验证码错误");
            });
        }
      });
    }
  }
};
</script>
<style scoped lang='less'>
.container-login {
  width: 100%;
  height: 100%;
  background: url(../../assets/login_bg.jpg) no-repeat center / cover;
  position: absolute;
  left: 0;
  top: 0;
  .el-card {
    width: 400px;
    height: 350px;
    position: absolute;
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);
    .logo {
      width: 200px;
      display: block;
      margin: 0 auto 20px;
    }
  }
}
</style>