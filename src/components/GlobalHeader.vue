<template>
  <div id="globalHeader">
    <a-row :wrap="false">
      <a-col flex="200px">
        <RouterLink to="/">
          <div class="title-bar">
            <img class="logo" src="../assets/logo.png" alt="logo" />
            <div class="title">Ymage</div>
          </div>
        </RouterLink>
      </a-col>
      <a-col flex="auto">
        <a-menu
          v-model:selectedKeys="current"
          mode="horizontal"
          :items="items"
          @click="doMenuClick"
        />
      </a-col>
      <a-col flex="120px">
        <div class="user-login-status">
          <div v-if="loginUserStore.loginUser.id">
            {{ loginUserStore.loginUser.userName ?? 'Unknown' }}
          </div>
          <div v-else>
            <a-button type="primary" href="/user/login">Login</a-button>
          </div>
        </div>
      </a-col>
    </a-row>
  </div>
</template>

<script lang="ts" setup>
import { h, onMounted, ref } from 'vue'
import { HomeOutlined } from '@ant-design/icons-vue'
import type { MenuProps } from 'ant-design-vue'
import { useRouter } from 'vue-router'
import { useLoginUserStore } from '@/stores/useLoginUserStore'

const items = ref<MenuProps['items']>([
  {
    key: '/',
    icon: () => h(HomeOutlined),
    label: 'Home',
    title: 'Home',
  },
  {
    key: 'others',
    label: h('a', { href: 'https://github.com/yngcy', target: '_blank' }, 'Github'),
    title: 'yngcy',
  },
])

const router = useRouter()
const loginUserStore = useLoginUserStore()
const current = ref<string[]>([])

router.afterEach((to) => {
  current.value = [to.path]
})

onMounted(() => {
  current.value = [router.currentRoute.value.path]
  loginUserStore.fetchLoginUser()
})

const doMenuClick = ({ key }: { key: string }) => {
  if (key === 'others') {
    return
  }
  router.push({ path: key })
}
</script>

<style scoped>
.title-bar {
  display: flex;
  align-items: center;
}

.title {
  color: black;
  font-size: 18px;
  margin-left: 16px;
}

.logo {
  height: 48px;
}
</style>
