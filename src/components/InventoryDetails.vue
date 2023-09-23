<script setup>
import InventoryDeleteButton from '@/components/UI/InventoryDeleteButton.vue';

const props = defineProps(['id', 'description', 'image']);
</script>

<template>
  <img :src="image" alt="" class="inventory-details__image" />
  <div class="inventory-details__description">{{ description }}</div>
  <InventoryDeleteButton />
</template>

<style scoped>
.inventory-details__image {
  width: 130px;
  padding: 35px 15px 27px;
}
.inventory-details__description {
  margin-bottom: 18px;
  padding: 16px 0;

  color: #4d4d4d;
  border-top: 1px solid #4d4d4d;
  border-bottom: 1px solid #4d4d4d;
}
</style>
