<script setup>

</script>
<template>
  <section class="base-dashboard">
    <slot />
  </section>
</template>

<style scoped>
.base-dashboard {
  margin-bottom: 10px;
  margin-top: 10px;
  padding: 16px;
  border-radius: 12px;
  background: #ffffff;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.08);
}
</style>
