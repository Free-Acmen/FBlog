<template>
  <div class="auth-panel">
    <h2 class="auth-title">
      后台登录
    </h2>
    <p class="auth-desc">
      <a-input
        ref="username"
        v-model="username"
        size="large"
        placeholder="请输入用户名"
      />
    </p>
    <div class="auth-input">
      <a-input-password
        ref="input"
        v-model="pwd"
        size="large"
        placeholder="请输入密码"
        @keyup.enter="login"
      />
    </div>
    <a-button type="primary" :block="true" size="large" @click="login">
      登录
    </a-button>
  </div>
</template>

<script lang="ts">
import Vue from 'vue';
import md5 from 'blueimp-md5';
export default Vue.extend({
  data () {
    return {
      username: 'FreeAcmen',
      pwd: ''
    };
  },
  mounted () {
    (this.$refs.input as any).$children[0].focus();
  },
  methods: {
    login (this: any) {
      if (!this.pwd) {
        (this.$refs.input as any).$children[0].focus();
        return;
      }
      this.$auth
        .loginWith('local', {
          data: {
            username: this.username,
            password: md5(this.pwd)
          }
        })
        .catch(err => {
          console.error(err);
          this.$message.error('密码不正确！');
        });
    }
  }
});
</script>

<style scoped>
.auth-input {
  margin: 0 0 20px;
}
</style>
