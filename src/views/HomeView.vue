<script setup lang="ts">
import { ref, onMounted, onUnmounted } from 'vue'
import elvaraLogo from '@/assets/elvara.webp'

const animDone = ref(false)
const phase = ref<'idle' | 'running' | 'done'>('idle')
const featureVisible = ref([false, false, false, false])

interface DesktopEnv {
  id: string
  name: string
  desc: string
  gradient: string
  initials: string
}

const desktopEnvs: DesktopEnv[] = [
  { id: 'kde',          name: 'KDE Plasma',           desc: '成熟完善的桌面环境',                gradient: 'linear-gradient(135deg, #4da9f7, #2563eb)', initials: 'KD' },
  { id: 'gnome',        name: 'GNOME',                desc: '简洁现代的桌面环境',                gradient: 'linear-gradient(135deg, #e8a838, #d97706)', initials: 'GN' },
  { id: 'shorinniri',   name: 'Shorin Niri',          desc: 'Shorin 定制版 Niri 平铺式桌面',     gradient: 'linear-gradient(135deg, #7c3aed, #5b21b6)', initials: 'SN' },
  { id: 'minimalniri',  name: '极简版 Niri',           desc: '轻量级 Niri 平铺桌面',             gradient: 'linear-gradient(135deg, #a78bfa, #7c3aed)', initials: 'JN' },
  { id: 'minimallabwc', name: '极简版 Labwc',          desc: '轻量级 Labwc 堆叠桌面',            gradient: 'linear-gradient(135deg, #f59e0b, #d97706)', initials: 'JL' },
  { id: 'end4',         name: 'End4 Quickshell',      desc: 'End4 风格 Quickshell 桌面',        gradient: 'linear-gradient(135deg, #ec4899, #db2777)', initials: 'E4' },
  { id: 'dms',          name: 'DMS Quickshell',       desc: 'DMS 风格 Quickshell 桌面',         gradient: 'linear-gradient(135deg, #14b8a6, #0d9488)', initials: 'DM' },
  { id: 'caelestia',    name: 'Caelestia Quickshell', desc: 'Caelestia 风格 Quickshell 桌面',   gradient: 'linear-gradient(135deg, #6366f1, #4f46e5)', initials: 'CA' },
  { id: 'inir',         name: 'Inir Quickshell',      desc: 'Inir 风格 Quickshell 桌面',        gradient: 'linear-gradient(135deg, #f97316, #ea580c)', initials: 'IN' },
  { id: 'shorindms',    name: 'Shorin DMS',           desc: 'Shorin 定制 DMS 桌面',             gradient: 'linear-gradient(135deg, #06b6d4, #0891b2)', initials: 'SD' },
  { id: 'shorinnocniri',name: 'Shorin Noctalia',      desc: 'Shorin Noctalia 定制桌面',         gradient: 'linear-gradient(135deg, #8b5cf6, #6d28d9)', initials: 'NO' },
  { id: 'hyprniri',     name: 'Shorin HyprNiri',      desc: 'HyprNiri 混合平铺桌面',            gradient: 'linear-gradient(135deg, #d946ef, #c026d3)', initials: 'HN' },
  { id: 'none',         name: '仅基础系统',             desc: '不安装桌面环境',                   gradient: 'linear-gradient(135deg, #6b7280, #4b5563)', initials: '--' },
]

const features = [
  {
    icon: '🛠️',
    title: '现代化安装体验',
    desc: '受 ArchInsstall 启发开发的图形安装程序，告别繁琐的命令行配置。友好的交互界面引导你完成每一步，让 Arch Linux 的安装不再是门槛。',
  },
  {
    icon: '📦',
    title: '真正的开箱即用',
    desc: '基于SHORiN-KiWATA/shorin-arch-setup仓库打造的开箱即用的桌面环境，可在安装时随意选择想要的桌面环境。',
  },
  {
    icon: '🛒',
    title: '始终保持最新',
    desc: '你的电脑不应该被升级束缚。得益于 Arch Linux 的滚动更新，随时运行sudo pacman -Syu即可更新，没有烦人的更新提醒。',
  },
  {
    icon: '🧰',
    title: '内置运维工具箱',
    desc: '随附 Arch 工具箱，提供镜像源切换等常用功能，操作可视化、结果即时反馈。即便是 Linux 新手，也能轻松维护自己的系统。',
  },
]

onMounted(() => {
  document.body.style.overflow = 'hidden'
  setTimeout(() => {
    phase.value = 'running'
    setTimeout(() => {
      animDone.value = true
      setTimeout(() => {
        phase.value = 'done'
        document.body.style.overflow = ''
        setupFeatureObserver()
      }, 600)
    }, 1400)
  }, 200)
})

onUnmounted(() => {
  document.body.style.overflow = ''
})

function setupFeatureObserver() {
  const cards = document.querySelectorAll('.feature-card')
  const observer = new IntersectionObserver(
    (entries) => {
      entries.forEach((entry) => {
        if (entry.isIntersecting) {
          const idx = Number((entry.target as HTMLElement).dataset.index)
          setTimeout(() => { featureVisible.value[idx] = true }, idx * 150)
          observer.unobserve(entry.target)
        }
      })
    },
    { threshold: 0.2 }
  )
  cards.forEach((el) => observer.observe(el))
}
</script>

<template>
  <section class="hero" :class="{ 'scroll-locked': phase !== 'done' }">

    <div
      class="rainbow-glow"
      :class="{ active: phase === 'running' || phase === 'done' }"
    ></div>

    <div class="hero-center">
      <div class="hero-logo" :class="{ visible: phase === 'running' || phase === 'done' }">
        <img :src="elvaraLogo" alt="ElvaraOS" />
      </div>

      <div class="hero-text" :class="{ visible: phase === 'running' || phase === 'done' }">
        <h1>ElvaraOS</h1>
        <p>一个开箱即用的 Arch Linux 发行版</p>
      </div>

      <div class="hero-buttons" :class="{ visible: animDone }">
        <a href="/download" class="btn btn-primary">下载</a>
        <a href="/docs" class="btn btn-secondary">文档</a>
      </div>
    </div>
  </section>

  <section class="desktop-envs">
    <h2 class="section-title">支持的桌面环境</h2>
    <p class="section-subtitle">安装时可从以下 13 种环境中自由选择</p>
    <div class="envs-grid">
      <div
        v-for="env in desktopEnvs"
        :key="env.id"
        class="env-card"
        :class="{ muted: env.id === 'none' }"
      >
        <div class="env-icon" :style="{ background: env.gradient }">{{ env.initials }}</div>
        <div class="env-body">
          <span class="env-name">{{ env.name }}</span>
          <span class="env-desc">{{ env.desc }}</span>
        </div>
      </div>
    </div>
  </section>

  <section class="features">
    <div
      v-for="(f, i) in features"
      :key="i"
      class="feature-card"
      :class="{ visible: featureVisible[i] }"
      :data-index="i"
    >
      <span class="feature-icon">{{ f.icon }}</span>
      <h3 class="feature-title">{{ f.title }}</h3>
      <p class="feature-desc">{{ f.desc }}</p>
    </div>
  </section>
</template>

<style scoped>
.hero {
  position: relative;
  width: 100%;
  height: calc(100vh - 56px);
  background: #000;
  display: flex;
  align-items: center;
  justify-content: center;
  overflow: hidden;
}

.scroll-locked {
  overflow: hidden;
}

/* 背景彩虹光晕 */
.rainbow-glow {
  position: absolute;
  inset: -60px -60px;
  z-index: 0;
  opacity: 0;
  background: radial-gradient(circle at 50% 40%, rgba(42, 109, 217, 0.08) 0%, rgba(123, 47, 255, 0.05) 30%, transparent 70%);
  transition: opacity 1.5s ease 0.3s;
  pointer-events: none;
}
.rainbow-glow.active {
  opacity: 1;
}

.hero-center {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  width: 100%;
  height: 100%;
  padding: 24px 0 24px;
  gap: 16px;
  box-sizing: border-box;
}

.hero-text {
  flex-shrink: 0;
  text-align: center;
  opacity: 0;
  filter: blur(12px);
  transform: translateY(36px);
  transition: opacity 1s ease, filter 1s ease, transform 1s ease;
}
.hero-text.visible {
  opacity: 1;
  filter: blur(0);
  transform: translateY(0);
}
.hero-text h1 {
  font-size: clamp(2.2rem, 4.5vw, 3.6rem);
  font-weight: 700;
  background: linear-gradient(135deg, #6ab4ff 0%, #2a6dd9 50%, #0d2a6e 100%);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  line-height: 1.15;
  margin-bottom: 8px;
}
.hero-text p {
  font-size: clamp(0.95rem, 1.8vw, 1.2rem);
  background: linear-gradient(135deg, #8ecfff 0%, #3a80d9 100%);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

.hero-logo {
  display: flex;
  align-items: center;
  justify-content: center;
  opacity: 0;
  filter: blur(20px);
  transform: scale(0.7);
  transition: opacity 1.2s ease 0.1s, filter 1.2s ease 0.1s, transform 1.2s cubic-bezier(0.22, 1, 0.36, 1) 0.1s;
}
.hero-logo.visible {
  opacity: 1;
  filter: blur(0);
  transform: scale(1);
}
.hero-logo img {
  display: block;
  width: auto;
  height: clamp(120px, 28vh, 320px);
  max-width: 80vw;
  object-fit: contain;
  filter: drop-shadow(0 0 40px rgba(42, 109, 217, 0.3));
}

.hero-buttons {
  flex-shrink: 0;
  display: flex;
  gap: 16px;
  opacity: 0;
  transform: translateY(28px);
  transition: opacity 0.6s ease, transform 0.6s ease;
  pointer-events: none;
}
.hero-buttons.visible {
  opacity: 1;
  transform: translateY(0);
  pointer-events: auto;
}

.btn {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  padding: 12px 36px;
  border-radius: 999px;
  font-size: 15px;
  font-weight: 500;
  cursor: pointer;
  transition: transform 0.2s, opacity 0.2s;
  white-space: nowrap;
}
.btn:hover { transform: translateY(-2px); opacity: 0.9; }

.btn-primary {
  background: linear-gradient(135deg, #2a6dd9, #1a3fa8);
  color: #fff;
  border: none;
}
.btn-secondary {
  background: transparent;
  color: rgba(255, 255, 255, 0.85);
  border: 1px solid rgba(255, 255, 255, 0.3);
}
.btn-secondary:hover {
  border-color: rgba(255, 255, 255, 0.6);
  color: #fff;
}

/* ── 桌面环境区域 ── */
.desktop-envs {
  background: #000;
  padding: 80px 48px 100px;
  text-align: center;
}

.section-title {
  font-size: clamp(1.6rem, 2.8vw, 2.2rem);
  font-weight: 700;
  color: #fff;
  margin: 0 0 10px;
}

.section-subtitle {
  font-size: clamp(0.9rem, 1.4vw, 1.05rem);
  color: rgba(255, 255, 255, 0.45);
  margin: 0 0 48px;
}

.envs-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
  gap: 16px;
  max-width: 960px;
  margin: 0 auto;
}

.env-card {
  display: flex;
  align-items: center;
  gap: 14px;
  padding: 18px 20px;
  border-radius: 14px;
  background: rgba(255, 255, 255, 0.03);
  border: 1px solid rgba(255, 255, 255, 0.06);
  transition: background 0.2s, border-color 0.2s, transform 0.2s;
  text-align: left;
}
.env-card:hover {
  background: rgba(255, 255, 255, 0.06);
  border-color: rgba(255, 255, 255, 0.12);
  transform: translateY(-2px);
}
.env-card.muted {
  opacity: 0.5;
}

.env-icon {
  width: 40px;
  height: 40px;
  border-radius: 10px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 13px;
  font-weight: 700;
  color: #fff;
  flex-shrink: 0;
}

.env-body {
  display: flex;
  flex-direction: column;
  gap: 2px;
  min-width: 0;
}

.env-name {
  font-size: 14px;
  font-weight: 600;
  color: #fff;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.env-desc {
  font-size: 12px;
  color: rgba(255, 255, 255, 0.4);
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

/* ── Features 区域 ── */
.features {
  background: #000;
  padding: 80px 48px 120px;
  display: flex;
  flex-direction: column;
  gap: 0;
}

.feature-card {
  display: flex;
  flex-direction: column;
  gap: 16px;
  padding: 64px 48px;
  border-top: 1px solid rgba(255, 255, 255, 0.07);
  opacity: 0;
  transform: translateY(48px);
  transition: opacity 0.7s ease, transform 0.7s ease;
}
.feature-card.visible {
  opacity: 1;
  transform: translateY(0);
}

.feature-icon {
  font-size: 2.4rem;
  line-height: 1;
}

.feature-title {
  font-size: clamp(1.4rem, 2.5vw, 2rem);
  font-weight: 700;
  color: #fff;
  margin: 0;
}

.feature-desc {
  font-size: clamp(0.95rem, 1.5vw, 1.1rem);
  color: rgba(255, 255, 255, 0.55);
  line-height: 1.8;
  max-width: 680px;
  margin: 0;
}

@media (max-width: 640px) {
  .desktop-envs {
    padding: 48px 20px 60px;
  }
  .envs-grid {
    grid-template-columns: repeat(auto-fill, minmax(160px, 1fr));
    gap: 10px;
  }
  .env-card {
    padding: 14px 16px;
    gap: 12px;
  }
  .env-icon {
    width: 34px;
    height: 34px;
    font-size: 11px;
  }
  .env-name {
    font-size: 13px;
  }
  .env-desc {
    font-size: 11px;
  }
  .features {
    padding: 48px 20px 80px;
  }
  .feature-card {
    padding: 36px 0;
    gap: 12px;
  }
  .hero-buttons {
    gap: 12px;
  }
  .btn {
    padding: 11px 28px;
    font-size: 14px;
  }
}
</style>
