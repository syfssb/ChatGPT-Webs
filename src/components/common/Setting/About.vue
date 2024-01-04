<script setup lang='ts'>
import { computed, onMounted, ref } from 'vue'
import { NSpin } from 'naive-ui'
import pkg from '../../../../package.json'
import { fetchChatConfig } from '@/api'
import { useAuthStore } from '@/store'

interface ConfigState {
  timeoutMs?: number
  reverseProxy?: string
  apiModel?: string
  socksProxy?: string
  httpsProxy?: string
  usage?: string
}

const authStore = useAuthStore()

const loading = ref(false)

const config = ref<ConfigState>()

const isChatGPTAPI = computed<boolean>(() => !!authStore.isChatGPTAPI)

async function fetchConfig() {
  try {
    loading.value = true
    const { data } = await fetchChatConfig<ConfigState>()
    config.value = data
  }
  finally {
    loading.value = false
  }
}

onMounted(() => {
  fetchConfig()
})
</script>

<template>
  <NSpin :show="loading">
    <div class="p-4 space-y-4">
      <h2 class="text-xl font-bold">
       关于此项目
      </h2>
      <div class="p-2 space-y-2 rounded-md bg-neutral-100 dark:bg-neutral-700">
        <p>
          此站为我们主站
          <a
            class="text-blue-600 dark:text-blue-500"
            href="https://new.cherrychat.org"
            target="_blank"
          >
            樱桃茶 GPT
          </a>的免费体验站。本站功能相对简单粗暴，随来随用，希望大家喜欢。后台使用的是gpt-4模型。由于人数可能很多，也许会出现卡顿的情况。更多OpenAI 最新的模型，更平滑，流畅，稳定，定制化的功能请去咱们的主站。
          <a
            class="text-blue-600 dark:text-blue-500"
            href="https://new.cherrychat.org"
            target="_blank"
          >
            樱桃茶 GPT
          </a>
					樱桃茶 vip QQ群：694177779;
					群内每日随机赠送 10w token 对话币。 群内所有成员享受套餐6 折福利
🔥600 元，全年 GPT4无限使用套餐，限时出售中！
				</p>
      </div>
    </div>
  </NSpin>
</template>
