<script setup>
defineProps({
  searchCity: {
    type: String,
    default: '',
  },
})

const emit = defineEmits(['update-query'])

</script>
<template>
  <div class="search-bar">
    <h3>🔍 도시 검색</h3>
    <input type="text" placeholder="검색할 도시 이름 입력" :value="searchCity"
      @input="emit('update-query', $event.target.value)" />
    <p>검색 중인 도시: <span>{{ searchCity }}</span></p>
  </div>
</template>

<style scoped>
.search-bar input {
  width: 100%;
  padding: 10px 12px;
  border: 1px solid #d1d5db;
  border-radius: 8px;
  font-size: 0.95rem;
}

.search-bar p {
  margin: 10px 0 0;
  font-size: 0.85rem;
  color: #0f1010;
}

.search-bar p span {
  font-weight: 600;
}
</style>
