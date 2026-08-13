<script setup>
import { ref, computed, watch } from 'vue'
import axios from 'axios'

const TOUR_API_KEY = '6a953028-7753-4602-9216-b32db2fc5d10'
// 한국문화정보원_전국 시티투어 코스와 함께하는 맛집 정보 API
const RESTAURANT_API_URL = 'https://api.kcisa.kr/openapi/API_CNV_063/request'
const PAGE_SIZE = 5
// API의 totalCount가 지역 필터와 무관하게 매우 크게 내려와서 페이지 수를 적당히 제한
const MAX_PAGES = 10

const props = defineProps({
  cityName: {
    type: String,
    default: '',
  },
})

const isLoading = ref(false)
const errorMessage = ref('')
const restaurants = ref([])
const currentPage = ref(1)
const totalCount = ref(0)

const totalPages = computed(() =>
  Math.min(MAX_PAGES, Math.max(1, Math.ceil(totalCount.value / PAGE_SIZE))),
)

// OpenWeatherMap은 lang=kr이어도 도시명(name)은 영문으로 내려주므로 맛집 API 조회용 한글 지역명으로 변환
const EN_TO_KO_CITY = {
  seoul: '서울',
  busan: '부산',
  incheon: '인천',
  daegu: '대구',
  daejeon: '대전',
  gwangju: '광주',
  ulsan: '울산',
  sejong: '세종',
  suwon: '수원',
  yongin: '용인',
  goyang: '고양',
  seongnam: '성남',
  bucheon: '부천',
  ansan: '안산',
  cheongju: '청주',
  jeonju: '전주',
  pohang: '포항',
  changwon: '창원',
  gimhae: '김해',
  jeju: '제주',
  chuncheon: '춘천',
  gangneung: '강릉',
  yeosu: '여수',
  mokpo: '목포',
  gyeongju: '경주',
  andong: '안동',
}

const toKoreanAreaName = (cityName) => EN_TO_KO_CITY[cityName.toLowerCase()] ?? cityName

// 같은 (도시, 페이지) 조합은 재요청 없이 즉시 보여주기 위한 캐시 (응답이 10초 안팎으로 느림)
const restaurantCache = new Map()

async function fetchRestaurants(cityName, pageNo, clNm = '한식') {
  const cacheKey = `${cityName}|${pageNo}|${clNm}`
  if (restaurantCache.has(cacheKey)) {
    return restaurantCache.get(cacheKey)
  }

  const { data } = await axios.get(RESTAURANT_API_URL, {
    params: {
      serviceKey: TOUR_API_KEY,
      numOfRows: PAGE_SIZE,
      pageNo,
      areaNm: toKoreanAreaName(cityName),
      clNm,
    },
    headers: {
      Accept: 'application/json',
    },
    timeout: 15000,
  })

  const body = data?.response?.body
  const items = body?.items?.item ?? []
  const itemList = Array.isArray(items) ? items : [items]

  const result = {
    items: itemList.map((item) => ({
      contentid: item.rstrGidCd,
      title: item.rstrBhfNm ? `${item.rstrNm} (${item.rstrBhfNm})` : item.rstrNm,
      category: item.rstrClNm,
      addr1: item.rstrRoadAddr || item.rstrLnbrAddr,
      tel: item.reference,
    })),
    totalCount: Number(body?.totalCount ?? 0),
  }

  restaurantCache.set(cacheKey, result)
  return result
}

async function loadRestaurants() {
  if (!props.cityName) return

  isLoading.value = true
  errorMessage.value = ''

  try {
    const result = await fetchRestaurants(props.cityName, currentPage.value)
    restaurants.value = result.items
    totalCount.value = result.totalCount
  } catch (error) {
    errorMessage.value =
      error.code === 'ECONNABORTED'
        ? '맛집 정보 응답이 너무 지연되어 중단했습니다. 잠시 후 다시 시도해 주세요.'
        : '맛집 정보를 불러오지 못했습니다.'
  } finally {
    isLoading.value = false
  }
}

function goToPage(page) {
  if (page < 1 || page > totalPages.value) return
  currentPage.value = page
  loadRestaurants()
}

watch(
  () => props.cityName,
  () => {
    currentPage.value = 1
    loadRestaurants()
  },
  { immediate: true },
)
</script>

<template>
  <section class="tour-section">
    <h4>🍽️ 추천 맛집</h4>

    <div v-if="isLoading" class="tour-status">
      <p>맛집 정보를 불러오는 중입니다. (최대 10초 이상 걸릴 수 있어요)</p>
    </div>

    <div v-else-if="errorMessage" class="tour-status">
      <p>{{ errorMessage }}</p>
    </div>

    <ul v-else-if="restaurants.length" class="tour-card-list">
      <li v-for="restaurant in restaurants" :key="restaurant.contentid" class="tour-card">
        <p class="tour-card-title">
          {{ restaurant.title }}
          <span v-if="restaurant.category" class="tour-card-badge">{{ restaurant.category }}</span>
        </p>
        <p class="tour-card-desc">{{ restaurant.addr1 }}</p>
        <p v-if="restaurant.tel" class="tour-card-desc">☎ {{ restaurant.tel }}</p>
      </li>
    </ul>

    <p v-else class="tour-empty">추천 맛집 정보가 없습니다.</p>

    <div v-if="restaurants.length" class="tour-pagination">
      <button type="button" :disabled="currentPage <= 1" @click="goToPage(currentPage - 1)">
        이전
      </button>
      <span class="tour-pagination-info">{{ currentPage }} / {{ totalPages }}</span>
      <button
        type="button"
        :disabled="currentPage >= totalPages"
        @click="goToPage(currentPage + 1)"
      >
        다음
      </button>
    </div>
  </section>
</template>

<style scoped>
.tour-section {
  margin-top: 20px;
  padding: 18px 20px;
  border: 1px solid rgba(148, 163, 184, 0.2);
  border-radius: 18px;
  background: linear-gradient(180deg, #ffffff 0%, #f8fbff 100%);
  box-shadow: 0 14px 40px rgba(15, 23, 42, 0.08);
}

.tour-section h4 {
  margin: 0 0 14px;
  font-size: 1.05rem;
  font-weight: 700;
  color: #0f172a;
}

.tour-status p {
  margin: 0;
  color: #6b7280;
}

.tour-card-list {
  list-style: none;
  margin: 0;
  padding: 0;
  display: flex;
  flex-direction: column;
  gap: 8px;
}

.tour-card {
  padding: 10px 12px;
  border: 1px solid #e5e7eb;
  border-radius: 10px;
}

.tour-card-title {
  margin: 0 0 4px;
  font-weight: 600;
  font-size: 0.9rem;
}

.tour-card-desc {
  margin: 0;
  font-size: 0.8rem;
  color: #6b7280;
}

.tour-card-desc + .tour-card-desc {
  margin-top: 4px;
}

.tour-card-badge {
  display: inline-block;
  margin-left: 6px;
  padding: 1px 8px;
  border-radius: 999px;
  background: #eff6ff;
  color: #2563eb;
  font-size: 0.7rem;
  font-weight: 600;
}

.tour-empty {
  margin: 0;
  font-size: 0.85rem;
  color: #9ca3af;
}

.tour-pagination {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 14px;
  margin-top: 14px;
}

.tour-pagination button {
  padding: 6px 14px;
  border: 1px solid #e5e7eb;
  border-radius: 8px;
  background: #ffffff;
  color: #1f2937;
  font-size: 0.85rem;
  cursor: pointer;
}

.tour-pagination button:disabled {
  color: #9ca3af;
  cursor: not-allowed;
}

.tour-pagination-info {
  font-size: 0.85rem;
  color: #6b7280;
}
</style>
