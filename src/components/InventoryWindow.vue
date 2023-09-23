<script setup>
const props = defineProps(['isInventoryWindowVisible']);
const emit = defineEmits(['closeInventoryWindow']);

const closeInventoryWindow = () => {
  emit('closeInventoryWindow');
};

closeInventoryWindow();
</script>

<template>
  <div v-show="isInventoryWindowVisible" class="inventory-details">
    <img
      @click="closeInventoryWindow"
      :src="'../src/assets/images/pict4.png'"
      alt=""
      class="inventory-details__close"
    />
    <slot></slot>
  </div>
</template>

<style scoped>
.inventory-details {
  position: absolute;
  top: 0;
  right: 0;
  z-index: 999;

  width: 28%;
  height: 77.5%;
  padding: 18px 15px;

  text-align: center;

  border-left: 1px solid #4d4d4d;
  border-top-right-radius: 12px;
  border-bottom-right-radius: 12px;
  background: rgba(38, 38, 38, 0.5);
  backdrop-filter: blur(8px);

  transition: transform 0.3s ease-in-out;
}

.inventory-details__close {
  position: absolute;
  right: 3%;
  top: 1%;
  cursor: pointer;
}

@media (max-width: 858.98px) {
  .inventory-details {
    width: 96%;
    height: 88%;
  }
}

@media (max-width: 767.98px) {
  .inventory-details {
    width: 95%;
  }
}

@media (max-width: 575.98px) {
  .inventory-details {
    width: 93%;
    height: 92%;
  }
}
@media (max-width: 414.98px) {
  .inventory-details {
    width: 88%;
    height: 92.5%;
  }
}
</style>
