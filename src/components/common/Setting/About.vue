<script setup lang='ts'>
import { computed, onMounted, ref } from 'vue'
import { NSpin } from 'naive-ui'
//import pkg from '../../../../package.json'
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
        关于本站
      </h2>
      <div class="p-2 space-y-2 rounded-md bg-neutral-100 dark:bg-neutral-700">
        <p>
  此站为我们主站
  <a
    class="text-blue-600 dark:text-blue-500"
    href="https://new.cherrychat.org"
    target="_blank"
    rel="noopener noreferrer"
  >
    樱桃茶 GPT
  </a>的免费体验站。本站界面简洁，易于操作，我们希望大家喜欢。后台采用了GPT-4模型。由于访问量可能较大，可能会偶尔出现延迟。想要体验OpenAI最新模型的更流畅、稳定和个性化功能，请访问我们的主站<br>
  <a
    class="text-blue-600 dark:text-blue-500"
    href="https://new.cherrychat.org"
    target="_blank"
    rel="noopener noreferrer"
  >
    樱桃茶 GPT
  </a>。<br>
</p>
<p>
  樱桃茶 VIP QQ 群：694177779，群内每日随机赠送10万token对话币，所有成员享受套餐六折优惠。🔥限时特惠：600元全年GPT-4无限使用套餐！<br>
</p>

      </div>
      <p>{{ $t("setting.api") }}：{{ config?.apiModel ?? '-' }}</p>
      <p v-if="isChatGPTAPI">
        {{ $t("setting.monthlyUsage") }}：{{ config?.usage ?? '-' }}
      </p>
      <p v-if="!isChatGPTAPI">
        {{ $t("setting.reverseProxy") }}：{{ config?.reverseProxy ?? '-' }}
      </p>
      <p>{{ $t("setting.timeout") }}：{{ config?.timeoutMs ?? '-' }}</p>
      <p>{{ $t("setting.socks") }}：{{ config?.socksProxy ?? '-' }}</p>
      <p>{{ $t("setting.httpsProxy") }}：{{ config?.httpsProxy ?? '-' }}</p>
    </div>
  </NSpin>
</template>
