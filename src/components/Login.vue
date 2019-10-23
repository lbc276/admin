<template>
  <div class="login">
    <el-form ref="form" :rules="rules" :model="form" label-width="80px">
      <img src="../assets/avatar.jpg" class="logo">
      <el-form-item label="用户名" prop="username">
        <el-input v-model="form.username" placeholder="亲输入用户名"></el-input>
      </el-form-item>
      <el-form-item label="密码" prop="password">
        <el-input v-model="form.password" placeholder="亲输入密码"></el-input>
      </el-form-item>
      <el-form-item>
        <el-button @click="login" type="primary" class="loginBtn" >登录</el-button>
        <el-button @click="reset">重置</el-button>
      </el-form-item>
    </el-form>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  data () {
    return {
      form: {
        username: '',
        password: ''
      },
      rules: {
        username: [
          { required: true, message: '请输入用户名', trigger: ['change', 'blur'] },
          { min: 3, max: 12, message: '长度在 3 到 12 个字符', trigger: ['change', 'blur'] }
        ],
        password: [
          { required: true, message: '请输入密码', trigger: ['change', 'blur'] },
          { min: 3, max: 12, message: '长度在 3 到 12 个字符', trigger: ['change', 'blur'] }
        ]
      }
    }
  },
  methods: {
    login () {
      this.$refs.form.validate(isValid => {
        if (!isValid) return
        axios.post('http://localhost:8888/api/private/v1/login', this.form).then(res => {
          const { meta } = res.data
          if (meta.status === 200) {
            this.$message({
              duration: 1000,
              message: meta.msg,
              type: 'success'
            })
            this.$router.push('/index')
          } else {
            this.$message({
              duration: 1000,
              message: meta.msg,
              type: 'error'
            })
          }
        })
      })
    },
    reset () {
      this.$refs.form.resetFields()
    }
  }

}
</script>

<style lang="less">
.login {
  width: 100%;
  height:100%;
  background-color: #2d434c;
  overflow: hidden;
  .el-form {
    margin: 0 auto;
    margin-top:200px;
    width:400px;
    background-color: #fff;
    padding: 20px;
    padding-top: 70px;
    border-radius: 20px;
    position:relative;
    .logo {
      border-radius: 50%;
      position: absolute;
      left:50%;
      transform: translateX(-50%);
      top:-75px;
      border: 5px solid #fff;
    }
    .loginBtn {
        margin-right: 70px;
      }
  }
}
</style>
