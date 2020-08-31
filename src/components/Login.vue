<template>
  <div class="login_container">
    <div class="login_box">
      <div class="login_header">
        <h3>用户登录</h3>
      </div>
      <div class="login_from">
        <el-form
          :model="loginForm"
          :rules="rules"
          ref="loginFormRef"
          label-position="right"
          status-icon
          :hide-required-asterisk="true"
        >
          <el-form-item label="用户名" prop="username">
            <el-input v-model="loginForm.username"></el-input>
          </el-form-item>
          <el-form-item label="密码" prop="password">
            <el-input v-model="loginForm.password"></el-input>
          </el-form-item>
          <el-form-item class="btns">
            <el-button type="primary" @click="login()">登录</el-button>
          </el-form-item>
        </el-form>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      loginForm: {
        username: '',
        password: ''
      },
      rules: {
        username: [
          { required: true, message: '请输入用户名', trigger: 'blur' },
          { min: 3, max: 5, message: '长度在 3 到 5 个字符', trigger: 'blur' }
        ],
        password: [
          { required: true, message: '请输入密码', trigger: 'blur' },
          { min: 6, max: 14, message: '长度在 6 到 14 个字符', trigger: 'blur' }
        ]
      }
    }
  },
  methods: {
    login () {
      this.$refs.loginFormRef.validate(async valid => {
        if (!valid) return

        const { data: res } = await this.$http.post('login', this.loginForm)

        console.log(res)
        if (res.meta.status === 200) {
          this.$message.success({ message: '登录成功', duration: 1000 })
          window.sessionStorage.setItem('token', res.data.tpken)
          this.$router.push('/home')
        } else {
          this.$message.error({ message: '登录失败', duration: 2000 })
        }
      })
    }
  }
}
</script>

<style lang="less" scoped>
.login_container {
  height: 100%;
  background-color: #2b4b6b;
}

.login_box {
  width: 450px;
  height: 330px;
  background-color: #ffffff;
  border-radius: 3px;
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
  .login_header {
    margin-left: 30px;
  }
}

.login_from {
  width: 400px;
  margin-left: 30px;
}

.btns .el-button {
  width: 400px;
}
</style>
