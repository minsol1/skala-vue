<script setup>
import { ref, computed, watch, watchEffect } from 'vue'
import { useRouter } from 'vue-router'
import axios from 'axios'
import BaseDashboardCard from '../components/exercise/BaseDashboardCard.vue'
import SearchBar from '../components/exercise/SearchBar.vue'
import WeatherCard from '../components/exercise/WeatherCard.vue'
import { useConfigStore } from '../stores/configStore'

const router = useRouter()
const configStore = useConfigStore()

const API_KEY = 'a4a7544409ce3ab9eed3f86e912a2c64'

const createURL = (cityName) =>
  `https://api.openweathermap.org/data/2.5/weather?q=${encodeURIComponent(cityName)}&appid=${API_KEY}&units=metric`
const weatherList = ref([])

const searchCity = ref('')
const statusMessage = ref('카드를 클릭하거나 검색해 보세요.')

const filteredWeatherList = computed(() => weatherList.value)

const displayWeatherList = computed(() => {
  return filteredWeatherList.value.map((weather) => {
    const displayTemp =
      configStore.unit === 'fahrenheit' ? Math.round((weather.temp * 9) / 5 + 32) : weather.temp

    return {
      ...weather,
      displayTemp,
    }
  })
})
function selectCity(weather) {
  statusMessage.value = `${weather.name}이 선택되었습니다.`
}

function showDetail(weather) {
  router.push(`/weather/${encodeURIComponent(weather.name)}`)
}

async function fetchWeatherByCity(cityName) {
  if (!cityName.trim()) {
    weatherList.value = []
    return
  }

  try {
    const response = await axios.get(createURL(cityName))
    const currentWeather = response.data

    weatherList.value = [
      {
        id: currentWeather.id,
        name: currentWeather.name,
        temp: currentWeather.main?.temp ?? 0,
        status: currentWeather.weather?.[0]?.description ?? '맑음',
      },
    ]
  } catch {
    weatherList.value = []
  }
}

watch(searchCity, (value) => {
  fetchWeatherByCity(value)
})

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
        :filtered-weather-list="displayWeatherList"
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
