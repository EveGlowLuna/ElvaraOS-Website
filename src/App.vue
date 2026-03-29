<script setup lang="ts">
import { RouterView } from 'vue-router'
import NavBar from './components/NavBar.vue'
import FooterBar from './components/FooterBar.vue'
</script>

<template>
  <NavBar />
  <main class="main-content">
    <RouterView />
  </main>
  <FooterBar />
</template>

<style scoped>
.main-content {
  flex: 1;
  display: flex;
  flex-direction: column;
  padding-top: 56px; /* 为固定导航栏预留空间 */
}
</style>
