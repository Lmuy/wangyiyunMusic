<template>
  <div class="loginwrap">
    <div class="formbox">
      <van-field v-model="loginForm.phone" label="手机号码" placeholder="请输入手机号码" />
      <van-field v-model="loginForm.password" label="密码" placeholder="请输入密码" />
    </div>
    <van-button class="lgBtn" type="primary" color="#fff" size="large" @click="login">登录</van-button>
    <van-button class="lgBtn" type="primary" color="#fff" size="large" @click="goregister">没有账号？去注册</van-button>
  </div>
</template>
<script lang="ts">
import { defineComponent, onMounted, reactive } from 'vue'
import { useRouter } from 'vue-router'
import { Toast } from 'vant';
import { useStore } from 'vuex';
import { login_cellphone } from '@/api/index'

export default defineComponent({
  name: 'Login',
  setup() {
    const router = useRouter();
    const store = useStore();
    let loginForm = reactive({
      phone: '',
      password: ''
    })
    onMounted(() => {

    })
    let methods:IMethods = {
      // 去注册
      goregister: (): void => {
        router.push('/register')
      },
      // 手机号码登录
      login: (): void => {
        if(!loginForm.phone || !loginForm.password){
          Toast('请输入完整信息');
          return
        }
        login_cellphone(loginForm).then(res=>{
          console.log('111',res)
          if(res.data.code === 200){
            store.commit('SET_USERINFO',res.data);
            router.push('/home');
          }else{
            if(res.data.message){
              Toast(res.data.message)
            }
          }
        });
      }
    }
    return {
      ...methods,
      loginForm
    }
  },
})
</script>
<style lang="less" scoped>
.loginwrap{
  width:100%;
  height:100%;
  padding:.6rem 0;
  box-sizing: border-box;
  // background: red;
  background: red;
  display: flex;
  flex-direction: column;
  justify-content: flex-end;
  align-items: center;
  .formbox{
    margin-bottom: 2rem;
  }
  .rgBtn,.lgBtn{
    width:80%;
    height:.9rem;
    margin:.2rem 0;
    // background: #fff;
    color:red !important;
    border-radius: .4rem;
    font-size: .3rem;
    display: flex;
    justify-content: center;
    align-items: center;
    font-weight: bold;
    letter-spacing: .06rem;
  }
}
</style>
