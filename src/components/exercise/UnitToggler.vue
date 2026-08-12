<script setup>
import { useConfigStore } from '../../stores/configStore'

const configStore = useConfigStore()
</script>

<template>
  <button type="button" class="unit-toggle-btn" @click="configStore.toggleUnit()">
    단위변경
  </button>
</template>

<style scoped>
.unit-toggle-btn {
  padding: 8px 12px;
  border: 0;
  border-radius: 8px;
  background: #31c172;
  color: #fff;
  font-weight: 700;
  cursor: pointer;
}
</style>