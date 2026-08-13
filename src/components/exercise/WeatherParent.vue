<script setup>
import { ref, computed, watch, watchEffect } from 'vue'
import BaseDashboardCard from './BaseDashboardCard.vue'
import SearchBar from './SearchBar.vue'
import WeatherCard from './WeatherCard.vue'

const weatherList = ref([
  { id: 'city_01', name: '서울', temp: 28, status: '맑음' },
  { id: 'city_02', name: '수원', temp: 24, status: '비' },
  { id: 'city_03', name: '부산', temp: 26, status: '구름' },
])

const searchCity = ref('')
const statusMessage = ref('카드를 클릭하거나 검색해 보세요.')

const filteredWeatherList = computed(() =>
  weatherList.value.filter((weather) => weather.name.includes(searchCity.value)),
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
  console.log(
    `🤖 [watchEffect 자동 호출] 현재 검색어 '${searchCity.value}'에 매칭되는 API 데이터를 필터링합니다. `,
  )
})
</script>
<template>
  <div class="weather-parent">
    <BaseDashboardCard>
      <SearchBar :search-city="searchCity" @update-query="searchCity = $event" />
    </BaseDashboardCard>

    <BaseDashboardCard>
      <h3>📋 지역별 날씨 현황</h3>
      <WeatherCard
        :filtered-weather-list="filteredWeatherList"
        @select-card="selectCity"
        @detail="showDetail"
      />
      <p v-if="filteredWeatherList.length === 0">😭 검색 결과와 일치하는 도시가 없습니다.</p>
    </BaseDashboardCard>

    <p class="status-bar">{{ statusMessage }}</p>
  </div>
</template>

<style scoped>
.weather-parent {
  max-width: 480px;
  margin: 0 auto;
  padding: 24px;
  border-radius: 16px;
  background: #f5f8fc;
  color: #1f2937;
}

.weather-parent h3 {
  margin: 0 0 10px;
  font-size: 1rem;
  color: #374151;
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
