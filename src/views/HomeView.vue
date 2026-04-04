<script setup lang="ts">
import { ref, onMounted, onUnmounted } from 'vue'
import osifImg from '@/assets/osif.webp'

const animDone = ref(false)
const phase = ref<'idle' | 'running' | 'done'>('idle')
const featureVisible = ref([false, false, false, false])

const heroImgRef = ref<HTMLImageElement | null>(null)
const glowRingRef = ref<HTMLDivElement | null>(null)

const features = [
  {
    icon: '🛠️',
    title: '现代化安装体验',
    desc: '受 ArchInsstall 启发开发的图形安装程序，告别繁琐的命令行配置。友好的交互界面引导你完成每一步，让 Arch Linux 的安装不再是门槛。',
  },
  {
    icon: '📦',
    title: '真正的开箱即用',
    desc: '预装中文输入法与精选 GNOME 插件，系统装好即可上手。借助 GNOME 强大的扩展生态，你可以随时按需调整桌面，打造专属工作流。',
  },
  {
    icon: '🛒',
    title: '始终保持最新',
    desc: '你的电脑不应该被升级束缚。得益于 Arch Linux 的滚动更新，随时运行sudo pacman -Syu即可更新，没有烦人的更新提醒。',
  },
  {
    icon: '🧰',
    title: '内置运维工具箱',
    desc: '随附 Arch 工具箱，提供镜像源切换、系统滚动更新等常用功能，操作可视化、结果即时反馈。即便是 Linux 新手，也能轻松维护自己的系统。',
  },
]

function syncGlow() {
  const img = heroImgRef.value
  const ring = glowRingRef.value
  if (!img || !ring) return
  const { offsetWidth: w, offsetHeight: h, offsetLeft: l, offsetTop: t } = img
  const inset = 18
  ring.style.width = `${w + inset * 2}px`
  ring.style.height = `${h + inset * 2}px`
  ring.style.left = `${l - inset}px`
  ring.style.top = `${t - inset}px`
}

let ro: ResizeObserver | null = null

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

  // 图片加载完后同步，并监听窗口 resize
  const img = heroImgRef.value
  if (img) {
    if (img.complete) syncGlow()
    else img.addEventListener('load', syncGlow)
  }
  ro = new ResizeObserver(syncGlow)
  if (img) ro.observe(img)
})

onUnmounted(() => {
  document.body.style.overflow = ''
  ro?.disconnect()
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
  <!-- hero 区域：占导航栏以下的整个视口高度 -->
  <section class="hero" :class="{ 'scroll-locked': phase !== 'done' }">

    <!-- 动态彩虹光晕：绝对定位，跟图片同层同尺寸 -->
    <div
      class="rainbow-glow"
      :class="{ active: phase === 'running' || phase === 'done' }"
    ></div>

    <div class="hero-center">
      <!-- 文字上浮 -->
      <div class="hero-text" :class="{ visible: phase === 'running' || phase === 'done' }">
        <h1>ElvaraOS</h1>
        <p>一个开箱即用的 Arch Linux 发行版</p>
      </div>

      <!-- 图片 + 光晕包裹层 -->
      <div class="img-glow-wrap" :class="{ visible: phase === 'running' || phase === 'done' }">
        <div class="glow-ring" ref="glowRingRef" :class="{ active: phase === 'running' || phase === 'done' }"></div>
        <img :src="osifImg" alt="ElvaraOS" class="hero-img" ref="heroImgRef" />
      </div>

      <!-- 按钮 -->
      <div class="hero-buttons" :class="{ visible: animDone }">
        <a href="/download" class="btn btn-primary">下载</a>
        <a href="/docs" class="btn btn-secondary">文档</a>
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

/* 居中容器：纵向 flex，图片区域 flex:1 吃掉剩余空间 */
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

/* 文字：固定自然高度 */
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

/* 图片 + 光晕：flex:1 撑满剩余高度，position:relative 作为光圈的定位基准 */
.img-glow-wrap {
  position: relative;
  flex: 1;
  min-height: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  opacity: 0;
  filter: blur(20px);
  transition: opacity 1.2s ease 0.1s, filter 1.2s ease 0.1s;
}
.img-glow-wrap.visible {
  opacity: 1;
  filter: blur(0);
}

/* 图片：高度撑满容器，宽度自适应，跟随窗口缩放 */
.hero-img {
  position: relative;
  z-index: 2;
  height: 100%;
  width: auto;
  max-width: 90vw;
  display: block;
  border-radius: 12px;
  transform: scale(0.72);
  transition: transform 1.2s cubic-bezier(0.22, 1, 0.36, 1) 0.1s;
}
.img-glow-wrap.visible .hero-img {
  transform: scale(1);
}

/* 竖屏：图片改为宽度驱动，限制高度防止撑满屏幕 */
@media (orientation: portrait) {
  .hero-img {
    width: 90vw;
    height: auto;
    max-width: 90vw;
    max-height: 55vh;
  }
}

/* 动态彩虹光晕：JS 控制 width/height/left/top，精确贴合图片渲染尺寸 */
.glow-ring {
  position: absolute;
  z-index: 1;
  border-radius: 18px;
  opacity: 0;
  transform: scale(0.85);
  background: conic-gradient(
    from 0deg,
    #ff0066, #ff6b00, #ffe600,
    #00ff88, #00cfff, #7b2fff, #ff0066
  );
  filter: blur(22px);
  transition: opacity 1s ease 0.3s, transform 1s ease 0.3s;
}
.glow-ring.active {
  opacity: 0.85;
  transform: scale(1);
  animation: rainbow-spin 4s linear infinite;
}

@keyframes rainbow-spin {
  from { filter: blur(22px) hue-rotate(0deg); }
  to   { filter: blur(22px) hue-rotate(360deg); }
}

/* 按钮：固定自然高度 */
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
