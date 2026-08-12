<template>
  <div class="app-shell">
    <header class="topbar">
      <nav class="nav-links">
        <RouterLink to="/">날씨 대시보드</RouterLink>
        <RouterLink to="/about">서비스 소개</RouterLink>
      </nav>
      <UnitToggler />
    </header>

    <main>
      <RouterView />
    </main>
  </div>
</template>

<script setup>
import UnitToggler from './components/exercise/UnitToggler.vue'
</script>

<style scoped>
.app-shell {
  min-height: 100vh;
  padding: 16px;
  background: #eef6ff;
}

.topbar {
  max-width: 960px;
  margin: 0 auto 16px;
  padding: 12px 16px;
  border-radius: 12px;
  background: #fff;
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: 12px;
}

.nav-links {
  display: flex;
  gap: 10px;
}

.nav-links a {
  text-decoration: none;
  color: #334155;
  font-weight: 700;
}
</style>
