<script setup>
import { ref } from 'vue'

// 响应式数据
const healthData = ref(null)
const clientData = ref(null)
const loading = ref(false)
const errorMsg = ref('')

// 后端接口地址（本地开发阶段直连）
const MAIN_API = ''
const CLIENT_API = ''

// 调用主服务 /health
async function fetchHealth() {
  loading.value = true
  errorMsg.value = ''
  try {
    const res = await fetch(`${MAIN_API}/health`)
    if (!res.ok) throw new Error(`HTTP ${res.status}`)
    healthData.value = await res.json()
  } catch (e) {
    errorMsg.value = '调用主服务 /health 失败：' + e.message
    healthData.value = null
  } finally {
    loading.value = false
  }
}

// 调用 client 服务 /api/client/info
async function fetchClient() {
  loading.value = true
  errorMsg.value = ''
  try {
    const res = await fetch(`${CLIENT_API}/api/client/info`)
    if (!res.ok) throw new Error(`HTTP ${res.status}`)
    clientData.value = await res.json()
  } catch (e) {
    errorMsg.value = '调用 client 服务失败：' + e.message
    clientData.value = null
  } finally {
    loading.value = false
  }
}
</script>

<template>
  <main>
    <header>
      <h1>Demo Frontend</h1>
      <p class="subtitle">
        一个极简前端，用于验证
        <code>frontend → main-service</code> 与
        <code>frontend → client-service</code> 两条链路。
      </p>
    </header>

    <div class="actions">
      <button class="btn main" @click="fetchHealth" :disabled="loading">
        {{ loading ? '请求中...' : '调用 主服务 /health' }}
      </button>
      <button class="btn client" @click="fetchClient" :disabled="loading">
        {{ loading ? '请求中...' : '调用 client 服务 /api/client/info' }}
      </button>
    </div>

    <div v-if="errorMsg" class="error">
      ⚠️ {{ errorMsg }}
    </div>

    <section v-if="healthData" class="card">
      <h2>主服务响应</h2>
      <pre>{{ JSON.stringify(healthData, null, 2) }}</pre>
    </section>

    <section v-if="clientData" class="card">
      <h2>Client 服务响应</h2>
      <pre>{{ JSON.stringify(clientData, null, 2) }}</pre>
    </section>

    <footer>
      <small>
        main: {{ MAIN_API }} &nbsp;|&nbsp; client: {{ CLIENT_API }}
      </small>
    </footer>
  </main>
</template>

<style scoped>
main {
  max-width: 780px;
  margin: 40px auto;
  padding: 24px;
  font-family: system-ui, -apple-system, "Helvetica Neue", sans-serif;
  color: #222;
}

header h1 {
  margin: 0;
  color: #2c3e50;
}

.subtitle {
  color: #666;
  font-size: 14px;
  line-height: 1.7;
}

code {
  background: #f5f5f5;
  padding: 2px 6px;
  border-radius: 3px;
  font-size: 12px;
}

.actions {
  display: flex;
  gap: 12px;
  margin: 24px 0;
  flex-wrap: wrap;
}

.btn {
  padding: 10px 18px;
  font-size: 14px;
  border: none;
  border-radius: 6px;
  color: white;
  cursor: pointer;
  transition: transform 0.1s, opacity 0.2s;
}

.btn:hover:not(:disabled) {
  transform: translateY(-1px);
}

.btn:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}

.btn.main {
  background: #42b883;
}

.btn.client {
  background: #3b82f6;
}

.error {
  background: #fee;
  color: #c00;
  padding: 12px 14px;
  border-radius: 6px;
  border-left: 4px solid #c00;
  margin: 12px 0;
  font-size: 14px;
}

.card {
  background: #fafafa;
  border: 1px solid #eee;
  border-radius: 6px;
  padding: 16px;
  margin: 16px 0;
}

.card h2 {
  margin-top: 0;
  font-size: 16px;
  color: #2c3e50;
}

pre {
  background: #272822;
  color: #f8f8f2;
  padding: 14px;
  border-radius: 4px;
  overflow: auto;
  font-size: 13px;
  line-height: 1.5;
  margin: 0;
}

footer {
  margin-top: 28px;
  padding-top: 16px;
  border-top: 1px solid #eee;
  color: #999;
  font-size: 12px;
  text-align: center;
}
</style>
