<template>
  <div class="login">
    <hm-header>登 录</hm-header>
    <hm-logo></hm-logo>

    <van-form class="form" @submit="onSubmit">
      <van-field
        v-model="username"
        placeholder="用户名"
        :rules="rules.username"
      />
      <van-field
        v-model="password"
        type="password"
        placeholder="密码"
        :rules="rules.password"
      />
      <div style="margin: 16px;">
        <van-button round block type="info" class="btn" native-type="submit">
          登 录
        </van-button>
      </div>
    </van-form>
    <p class="link">
      没有账号？去 <router-link to="/register">注册</router-link>
    </p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      username: '',
      password: '',
      rules: {
        username: [
          { required: true, message: '请填写用户名', trigger: 'onChange' },
          {
            pattern: /^\d{5,11}$/,
            message: '用户名只能是5-11位数字',
            trigger: 'onChange'
          }
        ],
        password: [
          { required: true, message: '请填写密码', trigger: 'onChange' },
          {
            pattern: /^\d{3,9}$/,
            message: '密码只能是3-9位数字',
            trigger: 'onChange'
          }
        ]
      }
    }
  },
  created() {
    const { username, password } = this.$route.params
    this.username = username
    this.password = password
  },
  methods: {
    async onSubmit() {
      const res = await this.$axios.post('/login', {
        username: this.username,
        password: this.password
      })
      const { statusCode, message, data } = res.data
      if (statusCode === 200) {
        this.$toast.success(message)
        this.$router.push('/')
        localStorage.setItem('token', data.token)
      } else {
        this.$toast.fail(message)
      }
    }
  }
}
</script>

<style lang="less" scoped>
.login {
  .btn {
    margin-top: 40px;
  }
  .link {
    float: right;
    margin: 16px 20px 0 0;
    a {
      color: crimson;
    }
  }
  .van-cell {
    border-radius: 25px;
    width: 343px;
    margin: 10px auto;
  }
}
</style>
