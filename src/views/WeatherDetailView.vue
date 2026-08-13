<script setup>
import { ref, onMounted } from 'vue'
import axios from 'axios'
import { useRoute, useRouter } from 'vue-router'
import { useConfigStore } from '../stores/configStore'

const route = useRoute()
const router = useRouter()
const configStore = useConfigStore()

const API_KEY = 'a4a7544409ce3ab9eed3f86e912a2c64'

const cityData = ref(null)
const isLoading = ref(true)
const errorMessage = ref('')

const displayTemp = (temp) =>
  configStore.unit === 'fahrenheit' ? Math.round((temp * 9) / 5 + 32) : temp

onMounted(async () => {
  try {
    const cityName = decodeURIComponent(route.params.cityName)
    const response = await axios.get(
      `https://api.openweathermap.org/data/2.5/weather?q=${encodeURIComponent(cityName)}&appid=${API_KEY}&units=metric&lang=kr`
    )

    const currentWeather = response.data

    cityData.value = {
      name: currentWeather.name,
      temp: currentWeather.main?.temp ?? 0,
      status: currentWeather.weather?.[0]?.description ?? '맑음',
      humidity: `${currentWeather.main?.humidity ?? 0}%`,
      wind: `${currentWeather.wind?.speed ?? 0}m/s`,
    }
  } catch {
    errorMessage.value = '해당 지역의 상세 데이터를 불러오지 못했습니다.'
  } finally {
    isLoading.value = false
  }
})
</script>

<template>
  <div class="detail-container">
    <h3>📊 지역별 상세 기상 관측 정보</h3>
    <hr />

    <div v-if="isLoading">
      <p>상세 날씨 정보를 불러오는 중입니다.</p>
    </div>

    <div v-else-if="cityData" class="info-card">
      <h4>📍 지정 지역: {{ cityData.name }}</h4>
      <p>
        실시간 기온: <strong>{{ displayTemp(cityData.temp) }}{{ configStore.unitSymbol }}</strong>
      </p>
      <p>기상 현황: {{ cityData.status }}</p>
      <p>대기 습도: {{ cityData.humidity }}</p>
      <p>현재 풍속: {{ cityData.wind }}</p>
    </div>
    <div v-else>
      <p>{{ errorMessage || '해당 지역의 상세 데이터 장부가 존재하지 않습니다.' }}</p>
    </div>

    <button @click="router.push('/')" class="back-btn">← 메인 대시보드로 돌아가기</button>
  </div>
</template>

<style scoped>
.detail-container {
  max-width: 520px;
  margin: 0 auto;
  padding: 28px 20px 36px;
  color: #1f2937;
}

.detail-container h3 {
  margin: 0 0 12px;
  font-size: 1.35rem;
  font-weight: 800;
  letter-spacing: -0.02em;
  color: #111827;
}

.detail-container hr {
  border: 0;
  height: 1px;
  margin: 0 0 18px;
  background: linear-gradient(90deg, rgba(59, 130, 246, 0.45), rgba(148, 163, 184, 0.15));
}

.detail-container>div {
  margin-bottom: 16px;
}

.detail-container p {
  margin: 0;
  line-height: 1.6;
}

.info-card {
  padding: 18px 20px;
  border: 1px solid rgba(148, 163, 184, 0.2);
  border-radius: 18px;
  background: linear-gradient(180deg, #ffffff 0%, #f8fbff 100%);
  box-shadow: 0 14px 40px rgba(15, 23, 42, 0.08);
}

.info-card h4 {
  margin: 0 0 12px;
  font-size: 1.05rem;
  font-weight: 700;
  color: #0f172a;
}

.info-card p+p {
  margin-top: 10px;
}

.back-btn {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
  width: 100%;
  margin-top: 6px;
  padding: 14px 18px;
  border: 0;
  border-radius: 14px;
  background: linear-gradient(135deg, #2563eb 0%, #0f766e 100%);
  color: #ffffff;
  font-weight: 700;
  cursor: pointer;
  box-shadow: 0 12px 24px rgba(37, 99, 235, 0.2);
  transition: transform 0.18s ease, box-shadow 0.18s ease, filter 0.18s ease;
}

.back-btn:hover {
  transform: translateY(-1px);
  box-shadow: 0 16px 30px rgba(37, 99, 235, 0.25);
  filter: brightness(1.02);
}

.back-btn:active {
  transform: translateY(0);
  box-shadow: 0 10px 20px rgba(37, 99, 235, 0.18);
}

.detail-container>div:first-of-type p,
.detail-container>div:last-of-type p {
  padding: 16px 18px;
  border-radius: 14px;
  background: #eff6ff;
  color: #1d4ed8;
  border: 1px solid rgba(59, 130, 246, 0.16);
}

@media (max-width: 640px) {
  .detail-container {
    padding: 20px 14px 28px;
  }

  .detail-container h3 {
    font-size: 1.2rem;
  }
}
</style>
