<template>
  <div id="userLoginPage">
    <h2 class="title">Ymage - User Login</h2>
    <div class="desc">AI image collaboration platform</div>

    <a-form :model="formState" name="basic" autocomplete="off" @finish="handleSubmit">
      <a-form-item
        name="userAccount"
        :rules="[{ required: true, message: 'Please input your account' }]"
      >
        <a-input v-model:value="formState.userAccount" placeholder="Please input account" />
      </a-form-item>

      <a-form-item
        name="userPassword"
        :rules="[
          { required: true, message: 'Please input password' },
          { min: 8, message: 'Password must be at least 8 chars' },
        ]"
      >
        <a-input-password
          v-model:value="formState.userPassword"
          placeholder="Please input password"
        />
      </a-form-item>

      <div class="tips">
        No account yet?
        <RouterLink to="/user/register">Register now</RouterLink>
      </div>

      <a-form-item>
        <a-button type="primary" html-type="submit" :loading="submitting" style="width: 100%"
          >Login</a-button
        >
      </a-form-item>
    </a-form>
  </div>
</template>

<script setup lang="ts">
import { reactive, ref } from 'vue'
import { message } from 'ant-design-vue'
import { userLoginUsingPost } from '@/api/userController'
import { useRouter } from 'vue-router'

const router = useRouter()
const submitting = ref(false)

const formState = reactive<API.UserLoginRequest>({
  userAccount: '',
  userPassword: '',
})

const handleSubmit = async () => {
  submitting.value = true
  try {
    const res = await userLoginUsingPost(formState)
    if (res.data.code === 0) {
      message.success('Login success')
      await router.push('/')
      return
    }
    message.error(res.data.message || 'Login failed')
  } finally {
    submitting.value = false
  }
}
</script>

<style scoped>
#userLoginPage {
  max-width: 420px;
  margin: 0 auto;
  padding: 24px;
  background: #fff;
  border-radius: 8px;
}

.title {
  margin-bottom: 8px;
}

.desc {
  color: #666;
  margin-bottom: 16px;
}

.tips {
  margin-bottom: 16px;
}
</style>
