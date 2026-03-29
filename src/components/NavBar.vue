<script setup lang="ts">
import { useRouter } from 'vue-router'
import elvaraImg from '@/assets/elvara.png'
import { GITHUB_REPO_URL } from '@/config'

const router = useRouter()
</script>

<template>
  <nav class="navbar">
    <div class="navbar-left" @click="router.push('/')">
      <img :src="elvaraImg" alt="ElvaraOS" class="nav-logo" />
    </div>
    <div class="navbar-right">
      <a :href="GITHUB_REPO_URL" target="_blank" rel="noopener">GitHub 仓库</a>
      <a href="/download">下载</a>
      <a href="/docs">文档</a>
    </div>
  </nav>
</template>

<style scoped>
.navbar {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  z-index: 100;
  height: 56px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0 24px;
  background: rgba(10, 10, 10, 0.45);
  backdrop-filter: blur(18px) saturate(180%);
  -webkit-backdrop-filter: blur(18px) saturate(180%);
  border-bottom: 1px solid rgba(255, 255, 255, 0.08);
}

.navbar-left {
  display: flex;
  align-items: center;
  cursor: pointer;
}

.nav-logo {
  height: 32px;
  width: auto;
  object-fit: contain;
}

.navbar-right {
  display: flex;
  align-items: center;
  gap: 28px;
}

.navbar-right a {
  font-size: 14px;
  color: rgba(255, 255, 255, 0.85);
  transition: color 0.2s;
  white-space: nowrap;
}

.navbar-right a:hover {
  color: #fff;
}
</style>
