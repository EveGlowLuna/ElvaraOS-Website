<script setup lang="ts">
import { ref } from 'vue'
import { useRouter } from 'vue-router'
import elvaraImg from '@/assets/elvara.webp'
import { GITHUB_REPO_URL } from '@/config'

const router = useRouter()
const menuOpen = ref(false)

function navigate(path: string) {
  menuOpen.value = false
  router.push(path)
}
</script>

<template>
  <nav class="navbar">
    <div class="navbar-left" @click="navigate('/')">
      <img :src="elvaraImg" alt="ElvaraOS" class="nav-logo" />
    </div>

    <!-- 桌面端链接 -->
    <div class="navbar-right desktop-links">
      <a :href="GITHUB_REPO_URL" target="_blank" rel="noopener">GitHub 仓库</a>
      <a href="/download">下载</a>
      <a href="/docs">文档</a>
    </div>

    <!-- 汉堡按钮（移动端） -->
    <button class="hamburger" :class="{ open: menuOpen }" @click="menuOpen = !menuOpen" aria-label="菜单">
      <span></span>
      <span></span>
      <span></span>
    </button>
  </nav>

  <!-- 移动端下拉菜单 -->
  <div class="mobile-menu" :class="{ open: menuOpen }" @click="menuOpen = false">
    <a :href="GITHUB_REPO_URL" target="_blank" rel="noopener">GitHub 仓库</a>
    <a href="/download">下载</a>
    <a href="/docs">文档</a>
  </div>
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

/* 汉堡按钮 */
.hamburger {
  display: none;
  flex-direction: column;
  justify-content: center;
  gap: 5px;
  width: 36px;
  height: 36px;
  background: none;
  border: none;
  cursor: pointer;
  padding: 4px;
}

.hamburger span {
  display: block;
  height: 2px;
  background: rgba(255, 255, 255, 0.85);
  border-radius: 2px;
  transition: transform 0.25s ease, opacity 0.25s ease;
}

.hamburger.open span:nth-child(1) { transform: translateY(7px) rotate(45deg); }
.hamburger.open span:nth-child(2) { opacity: 0; }
.hamburger.open span:nth-child(3) { transform: translateY(-7px) rotate(-45deg); }

/* 移动端下拉菜单 */
.mobile-menu {
  position: fixed;
  top: 56px;
  left: 0;
  right: 0;
  z-index: 99;
  background: rgba(10, 10, 10, 0.95);
  backdrop-filter: blur(18px);
  -webkit-backdrop-filter: blur(18px);
  border-bottom: 1px solid rgba(255, 255, 255, 0.08);
  display: flex;
  flex-direction: column;
  overflow: hidden;
  max-height: 0;
  transition: max-height 0.3s ease;
}

.mobile-menu.open {
  max-height: 200px;
}

.mobile-menu a {
  padding: 16px 24px;
  font-size: 15px;
  color: rgba(255, 255, 255, 0.85);
  border-bottom: 1px solid rgba(255, 255, 255, 0.06);
  transition: background 0.15s;
}

.mobile-menu a:last-child {
  border-bottom: none;
}

.mobile-menu a:hover {
  background: rgba(255, 255, 255, 0.05);
  color: #fff;
}

@media (max-width: 640px) {
  .desktop-links { display: none; }
  .hamburger { display: flex; }
}
</style>
