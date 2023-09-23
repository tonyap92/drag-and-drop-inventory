<script setup>
import { RouterView } from 'vue-router';
</script>

<template>
  <div class="wrapper">
    <div class="container">
      <RouterView v-slot="{ Component }">
        <keep-alive>
          <component :is="Component" />
        </keep-alive>
      </RouterView>
    </div>
  </div>
</template>

<style scoped>

.wrapper {
  max-width: 100%;
  min-width: 320px;
  width: 100%;
  margin: 0 auto;

  font-family: 'Inter', sans-serif !important;
  color: #000000;

  background-color: #000000;
  box-sizing: border-box;
  overflow: hidden;
}

.container {
  max-width: 859px;
  width: 100%;
  margin: 0 auto;
  position: relative;
}
</style>
