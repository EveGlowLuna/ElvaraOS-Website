<script setup lang="ts">
import { ref, onMounted, computed } from 'vue'
import { DOWNLOAD_BASE_URL, GITHUB_REPO_URL } from '@/config'

const version = ref('')      // e.g. "1.0"
const changelog = ref('')    // 第三行起的日志内容

onMounted(async () => {
  try {
    const res = await fetch('/publish_info.md')
    const text = await res.text()
    const lines = text.split('\n')

    // 第一行格式：# v1.0  →  取 v 后面的部分
    const match = lines[0].match(/^#\s*v(.+)/)
    if (match) version.value = match[1].trim()

    // 第三行到最后
    changelog.value = lines.slice(2).join('\n').trim()
  } catch (e) {
    console.error('Failed to load publish_info.md', e)
  }
})

const isoUrl = computed(
  () => `${DOWNLOAD_BASE_URL}/ElvaraOS-${version.value}-x86_64.iso`
)
</script>

<template>
  <div class="download-page">
    <h1 class="page-title">下载 ElvaraOS</h1>

    <div class="card">
      <!-- 左：版本 + 日志 -->
      <div class="card-info">
        <div class="version-badge" v-if="version">v{{ version }}</div>
        <div class="changelog" v-if="changelog">
          <p v-for="(line, i) in changelog.split('\n')" :key="i" class="log-line">
            {{ line }}
          </p>
        </div>
        <div v-if="!version" class="loading">加载中…</div>
      </div>

      <!-- 右：下载按钮 -->
      <div class="card-actions">
        <a
          :href="isoUrl"
          class="btn btn-primary"
          :class="{ disabled: !version }"
          :download="version ? `ElvaraOS-${version}-x86_64.iso` : undefined"
        >
          <span class="btn-icon">⬇</span>
          直接下载 ISO
          <span class="btn-sub" v-if="version">ElvaraOS-{{ version }}-x86_64.iso</span>
        </a>

        <a
          :href="GITHUB_REPO_URL"
          target="_blank"
          rel="noopener"
          class="btn btn-secondary"
        >
          <span class="btn-icon">
            <svg width="18" height="18" viewBox="0 0 24 24" fill="currentColor">
              <path d="M12 0C5.37 0 0 5.37 0 12c0 5.31 3.435 9.795 8.205 11.385.6.105.825-.255.825-.57 0-.285-.015-1.23-.015-2.235-3.015.555-3.795-.735-4.035-1.41-.135-.345-.72-1.41-1.23-1.695-.42-.225-1.02-.78-.015-.795.945-.015 1.62.87 1.845 1.23 1.08 1.815 2.805 1.305 3.495.99.105-.78.42-1.305.765-1.605-2.67-.3-5.46-1.335-5.46-5.925 0-1.305.465-2.385 1.23-3.225-.12-.3-.54-1.53.12-3.18 0 0 1.005-.315 3.3 1.23.96-.27 1.98-.405 3-.405s2.04.135 3 .405c2.295-1.56 3.3-1.23 3.3-1.23.66 1.65.24 2.88.12 3.18.765.84 1.23 1.905 1.23 3.225 0 4.605-2.805 5.625-5.475 5.925.435.375.81 1.095.81 2.22 0 1.605-.015 2.895-.015 3.3 0 .315.225.69.825.57A12.02 12.02 0 0 0 24 12c0-6.63-5.37-12-12-12z"/>
            </svg>
          </span>
          前往 GitHub 仓库
        </a>
      </div>
    </div>
  </div>
</template>

<style scoped>
.download-page {
  min-height: calc(100vh - 56px - 65px);
  background: #000;
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 72px 24px 80px;
}

.page-title {
  font-size: clamp(1.8rem, 3vw, 2.6rem);
  font-weight: 700;
  color: #fff;
  margin-bottom: 48px;
  text-align: center;
}

/* 卡片：自适应内容高度，最大宽度限制 */
.card {
  width: min(860px, 100%);
  background: rgba(255, 255, 255, 0.04);
  border: 1px solid rgba(255, 255, 255, 0.1);
  border-radius: 20px;
  display: flex;
  overflow: hidden;
}

/* 左侧：版本 + 日志 */
.card-info {
  flex: 1;
  padding: 40px 36px;
  display: flex;
  flex-direction: column;
  gap: 20px;
  border-right: 1px solid rgba(255, 255, 255, 0.08);
  overflow-y: auto;
  scrollbar-width: none;
}
.card-info::-webkit-scrollbar { display: none; }

.version-badge {
  display: inline-block;
  padding: 4px 14px;
  border-radius: 999px;
  background: rgba(42, 109, 217, 0.25);
  border: 1px solid rgba(42, 109, 217, 0.5);
  color: #6ab4ff;
  font-size: 0.9rem;
  font-weight: 600;
  width: fit-content;
}

.changelog {
  display: flex;
  flex-direction: column;
  gap: 6px;
}

.log-line {
  font-size: 0.92rem;
  color: rgba(255, 255, 255, 0.6);
  line-height: 1.7;
  margin: 0;
  white-space: pre-wrap;
}

.loading {
  color: rgba(255, 255, 255, 0.3);
  font-size: 0.9rem;
}

/* 右侧：按钮 */
.card-actions {
  width: 280px;
  flex-shrink: 0;
  padding: 40px 32px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  gap: 16px;
}

.btn {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 6px;
  padding: 18px 20px;
  border-radius: 14px;
  font-size: 15px;
  font-weight: 500;
  cursor: pointer;
  text-align: center;
  transition: transform 0.2s, opacity 0.2s;
  text-decoration: none;
}
.btn:hover { transform: translateY(-2px); opacity: 0.9; }
.btn.disabled { pointer-events: none; opacity: 0.4; }

.btn-icon {
  font-size: 1.4rem;
  display: flex;
  align-items: center;
}

.btn-sub {
  font-size: 11px;
  opacity: 0.6;
  word-break: break-all;
}

.btn-primary {
  background: linear-gradient(135deg, #2a6dd9, #1a3fa8);
  color: #fff;
  border: none;
}

.btn-secondary {
  background: transparent;
  color: rgba(255, 255, 255, 0.8);
  border: 1px solid rgba(255, 255, 255, 0.2);
}
.btn-secondary:hover {
  border-color: rgba(255, 255, 255, 0.5);
  color: #fff;
}
</style>
