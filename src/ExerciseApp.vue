<script setup>
// import WeatherParent from './components/exercise/WeatherParent.vue'
</script>

<template>
  <div class="app-container">
    <h1>⛅ 과제 : 날씨</h1>
    <hr />
    <!-- <WeatherParent /> -->
    <div class="dashboard-wrapper">
      <nav class="navigation-bar">
        <RouterLink to="/" class="nav-item">🌦️ 날씨 대시보드</RouterLink>
        <span class="divider">|</span>
        <RouterLink to="/about" class="nav-item">ℹ️ 서비스 소개</RouterLink>
      </nav>
      <UnitToggler />
      <main>
        <RouterView />
      </main>
    </div>
  </div>
</template>
