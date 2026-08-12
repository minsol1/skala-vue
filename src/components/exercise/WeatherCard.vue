<script setup>
defineProps({
  filteredWeatherList: {
    type: Array,
    default: () => [],
  },
})

const emit = defineEmits(['select-card', 'detail'])

</script>

<template>

  <section class="weather-list">

    <ul>
      <li v-for="weather in filteredWeatherList" :key="weather.id" class="weather-card"
        @click="emit('select-card', weather)">
        <div class="weather-info">
          <p class="city-name">{{ weather.name }} ({{ weather.status }})</p>
          <p class="temp">한낮 기온: {{ weather.displayTemp ?? weather.temp }}°C</p>
          <span v-if="weather.temp >= 25" class="status-badge hot">🔥 더움 (25도 이상)</span>
          <span v-else class="status-badge cool">❄️ 선선함 (25도 미만)</span>
        </div>
        <button type="button" @click.stop="emit('detail', weather)">상세보기</button>
      </li>
    </ul>

  </section>

</template>

<style scoped>
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
</style>

