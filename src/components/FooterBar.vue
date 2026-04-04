<script setup lang="ts">
import elvaraImg from '@/assets/elvara.webp'

const year = new Date().getFullYear()
</script>

<template>
  <footer class="footer">
    <div class="footer-left">
      <img :src="elvaraImg" alt="ElvaraOS" class="footer-logo" />
      <span class="footer-name">ElvaraOS</span>
    </div>
    <div class="footer-right">
      GPL-3.0 Licensed | Copyright {{ year }} EveGlow Works
    </div>
  </footer>
</template>

<style scoped>
.footer {
  background: #000;
  border-top: 1px solid rgba(255, 255, 255, 0.08);
  display: flex;
  align-items: center;
  justify-content: space-between;
  flex-wrap: wrap;
  gap: 8px;
  padding: 20px 28px;
  font-size: 13px;
  color: rgba(255, 255, 255, 0.5);
  flex-shrink: 0;
}

@media (max-width: 480px) {
  .footer {
    flex-direction: column;
    align-items: flex-start;
    padding: 16px 20px;
  }
}

.footer-left {
  display: flex;
  align-items: center;
  gap: 10px;
}

.footer-logo {
  height: 24px;
  width: auto;
  object-fit: contain;
  opacity: 0.8;
}

.footer-name {
  font-size: 14px;
  color: rgba(255, 255, 255, 0.7);
  font-weight: 500;
}

.footer-right {
  font-size: 13px;
}
</style>
