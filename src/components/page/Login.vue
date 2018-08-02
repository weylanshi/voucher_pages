<template>
    <div class="login-wrap">
        <div class="ms-title">捷鑫合同管理凭证生成系统</div>
        <div class="ms-login">
            <el-form :model="ruleForm" :rules="rules" ref="ruleForm" label-width="0px" class="demo-ruleForm">
                <el-form-item prop="username">
                    <el-input v-model="ruleForm.username" placeholder="用户名"></el-input>
                </el-form-item>
                <el-form-item prop="password">
                    <el-input type="password" placeholder="密码" v-model="ruleForm.password" @keyup.enter.native="submitForm('ruleForm')"></el-input>
                </el-form-item>
                <el-form-item>
                  <el-date-picker
                      v-model="ruleForm.loginDate"
                      type="datetime"
                      placeholder="选择日期时间"
                      align="right" class="datepicker">
                  </el-date-picker>
                </el-form-item>
                <div class="login-btn">
                    <el-button type="primary" @click="submitForm('ruleForm')">登录</el-button>
                </div>
                <p style="font-size:12px;line-height:30px;color:#999;">Tips : 用户名密码请联系管理员</p>
            </el-form>
        </div>
    </div>
</template>


<script>
export default {
  data: function() {
    return {
      ruleForm: {
        username: "",
        password: "",
        loginDate: ""
      },
      rules: {
        username: [
          { required: true, message: "请输入用户名", trigger: "blur" }
        ],
        password: [{ required: true, message: "请输入密码", trigger: "blur" }]
      }
    };
  },
  methods: {
    submitForm(formName) {
      this.$refs[formName].validate(valid => {
        if (valid) {
          this.$axios
            .post("/api/login", this.$qs.stringify(this.ruleForm))
            .then(res => {
              const { data, msg, status } = res.data;
              if (status === 200) {
                localStorage.setItem("ms_username", this.ruleForm.username);
                this.$router.push("/");
              } else {
                this.$message.error(msg);

                
              }
            });
        } else {
          this.$message.error("网络错误");
          return false;
        }
      });
    }
  }
};
</script>

<style scoped>
.login-wrap {
  position: relative;
  width: 100%;
  height: 100%;
}
.ms-title {
  position: absolute;
  top: 50%;
  width: 100%;
  margin-top: -230px;
  text-align: center;
  font-size: 30px;
  color: #fff;
}
.ms-login {
  position: absolute;
  left: 50%;
  top: 50%;
  width: 300px;
  height: 160px;
  margin: -150px 0 0 -190px;
  padding: 40px 40px 80px 40px;
  border-radius: 5px;
  background: #fff;
}
.datepicker.el-input{
   width: 300px;
}
.login-btn {
  text-align: center;
}
.login-btn button {
  width: 100%;
  height: 36px;
}
</style>