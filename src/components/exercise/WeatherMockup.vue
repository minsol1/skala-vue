<script setup>
import { ref, computed, watch, watchEffect } from 'vue'

const weatherList = ref([
  { id: 'city_01', name: '서울', temp: 28, status: '맑음' },
  { id: 'city_02', name: '수원', temp: 24, status: '비' },
  { id: 'city_03', name: '부산', temp: 26, status: '구름' },
])

const searchCity = ref('')
const statusMessage = ref('카드를 클릭하거나 검색해 보세요.')

const filteredWeatherList = computed(() =>
  weatherList.value.filter((weather) => weather.name.includes(searchCity.value))
)

function selectCity(weather) {
  statusMessage.value = `${weather.name}이 선택되었습니다.`
}

function showDetail(weather) {
  window.alert(`[${weather.name}]의 현재 날씨는 [${weather.status}] 상태입니다.`)
}

watch(statusMessage, (newValue) => {
  console.log(`🤖 [watch 감지] 상태 바 문구가 업데이트되었습니다 ->"${newValue}"`)
})

watchEffect(() => {
  console.log(`🤖 [watchEffect 자동 호출] 현재 검색어 '${searchCity.value}'에 매칭되는 API 데이터를 필터링합니다. `)
})
</script>
<template>
  <div class="weather-mockup">

    <section class="search-box">
      <h3>🔍 도시 검색</h3>
      <input type="text" placeholder="검색할 도시 이름 입력" :value="searchCity" @input="searchCity = $event.target.value" />
      <p>검색 중인 도시: <span>{{ searchCity }}</span></p>
    </section>

    <section class="weather-list">
      <h3>📋 지역별 날씨 현황</h3>
      <ul>
        <li v-for="weather in filteredWeatherList" :key="weather.id" class="weather-card" @click="selectCity(weather)">
          <div class="weather-info">
            <p class="city-name">{{ weather.name }} ({{ weather.status }})</p>
            <p class="temp">한낮 기온: {{ weather.temp }}°C</p>
            <span v-if="weather.temp >= 25" class="status-badge hot">🔥 더움 (25도 이상)</span>
            <span v-else class="status-badge cool">❄️ 선선함 (25도 미만)</span>
          </div>
          <button type="button" @click.stop="showDetail(weather)">상세보기</button>
        </li>
      </ul>
      <p v-if="filteredWeatherList.length === 0">😭 검색 결과와 일치하는 도시가 없습니다. </p>
    </section>

    <p class="status-bar">{{ statusMessage }}</p>
  </div>
</template>


<style scoped>
.weather-mockup {
  max-width: 480px;
  margin: 0 auto;
  padding: 24px;
  border-radius: 16px;
  background: #f5f8fc;
  color: #1f2937;
}

.weather-mockup h3 {
  margin: 0 0 10px;
  font-size: 1rem;
  color: #374151;
}

.search-box {
  margin-bottom: 20px;
  padding: 16px;
  border-radius: 12px;
  background: #ffffff;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.08);
}

.search-box input {
  width: 100%;
  padding: 10px 12px;
  border: 1px solid #d1d5db;
  border-radius: 8px;
  font-size: 0.95rem;
}

.search-box p {
  margin: 10px 0 0;
  font-size: 0.85rem;
  color: #0f1010;
}

.search-box p span {
  font-weight: 600;
}

.weather-list {
  padding: 16px;
  border-radius: 12px;
  background: #ffffff;
}

.weather-list ul {
  list-style: none;
  margin: 0;
  padding: 0;
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.weather-card {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 14px;
  border: 1px solid #e5e7eb;
  border-radius: 10px;
  cursor: pointer;
}

.weather-card:hover {
  background-color: #f0f6ff;
  box-shadow: 0 2px 6px rgba(0, 0, 0, 0.06);
}

.weather-info .city-name {
  margin: 0 0 4px;
  font-weight: 600;
  font-size: 1rem;
}

.weather-info .temp {
  margin: 0 0 6px;
  font-size: 0.85rem;
  color: #6b7280;
}

.status-badge {
  display: inline-block;
  padding: 2px 8px;
  border-radius: 999px;
  font-size: 0.75rem;
  font-weight: 600;
}

.status-badge.hot {
  background: #fee2e2;
  color: #dc2626;
}

.status-badge.cool {
  background: #dbeafe;
  color: #2563eb;
}

.weather-card button {
  margin-left: 12px;
  padding: 8px 14px;
  border: none;
  border-radius: 8px;
  background: #2c56b2;
  color: #ffffff;
  font-size: 0.85rem;
  cursor: pointer;
}

.weather-list>p {
  margin: 12px 0 0;
  text-align: center;
  color: #9ca3af;
  font-size: 0.9rem;
}

.status-bar {
  margin: 20px 0 0;
  padding: 12px;
  border-radius: 10px;
  background: #31c172;
  color: #f9fafb;
  text-align: center;
  font-size: 0.9rem;
}
</style>
