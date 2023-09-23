<script setup>
import { ref, computed, watch } from 'vue';
import { inventoryItems } from '@/data/inventory.js';

import InventoryWindow from '@/components/InventoryWindow.vue';
import InventoryDetails from '@/components/InventoryDetails.vue';

let draggingInventoryItem = null;
let targetInventoryItem = null;

const isInventoryWindowVisible = ref(true);
const clickedInventoryItem = ref(null);

const inventoryDetails = computed(() => {
  if (!clickedInventoryItem.value) {
    return null;
  }
  const index = findInventoryItemIndexById(clickedInventoryItem.value);
  if (index === -1) {
    return null;
  }
  return {
    id: inventoryItems.value[index].id,
    image: inventoryItems.value[index].image,
    description: inventoryItems.value[index].description,
  };
});

const canMoveInventoryItems = () => {
  return draggingInventoryItem && targetInventoryItem && inventoryItems.value;
};

const findInventoryItemIndexById = (id) => {
  return inventoryItems.value.findIndex((item) => item.id === id);
};

const swapInventoryItems = (indexDrag, indexTarget) => {
  [inventoryItems.value[indexDrag].image, inventoryItems.value[indexTarget].image] = [
    inventoryItems.value[indexTarget].image,
    inventoryItems.value[indexDrag].image,
  ];
  [inventoryItems.value[indexDrag].quantity, inventoryItems.value[indexTarget].quantity] = [
    inventoryItems.value[indexTarget].quantity,
    inventoryItems.value[indexDrag].quantity,
  ];
  [inventoryItems.value[indexDrag].description, inventoryItems.value[indexTarget].description] = [
    inventoryItems.value[indexTarget].description,
    inventoryItems.value[indexDrag].description,
  ];
  localStorage.setItem('inventoryItems', JSON.stringify(inventoryItems.value));
};

const handleInventoryDragStart = (event, item) => {
  draggingInventoryItem = item;
};

const handleInventoryDragOver = (event, item) => {
  event.preventDefault();
  targetInventoryItem = item;
};

const handleInventoryDrop = (event) => {
  event.preventDefault();
  if (canMoveInventoryItems()) {
    const indexDrag = findInventoryItemIndexById(draggingInventoryItem.id);
    const indexTarget = findInventoryItemIndexById(targetInventoryItem.id);
    swapInventoryItems(indexDrag, indexTarget);
    draggingInventoryItem = null;
    targetInventoryItem = null;
  }
};

const handleInventaryItemClick = (itemId) => {
  isInventoryWindowVisible.value = !isInventoryWindowVisible.value;
  clickedInventoryItem.value = itemId;
  if (itemId) {
    const index = findInventoryItemIndexById(itemId);
    if (index !== -1) {
      clickedInventoryItem.value = itemId;
    }
  }
};

const inventoryItemsFromLocalStorage = JSON.parse(localStorage.getItem('inventoryItems')) || [];
inventoryItems.value = inventoryItemsFromLocalStorage.length ? inventoryItemsFromLocalStorage : [];
</script>

<template>
  <main class="main">
    <div class="inventory">
      <div class="inventory-scoreboard">
        <div class="inventory-scoreboard__account">
          <div class="inventory-scoreboard__photo">
            <img :src="'../src/assets/images/pict1.png'" alt="pict1" />
          </div>
          <div class="inventory-scoreboard__info">
            <img :src="'../src/assets/images/pict2.png'" alt="pict2" />
          </div>
        </div>
        <div class="inventory-scoreboard__action" @drop="handleInventoryDrop" @dragover.prevent>
          <div
            v-for="item in inventoryItems"
            :key="item.id"
            class="inventory-scoreboard__item"
            @click="item.quantity && handleInventaryItemClick(item.id)"
            :draggable="item.id !== ''"
            @dragstart="handleInventoryDragStart($event, item)"
            @dragover="handleInventoryDragOver($event, item)"
          >
            <img :src="item.image" class="inventory-scoreboard__pict" />
            <div v-if="item.quantity" class="inventory-scoreboard__quantity">{{ item.quantity }}</div>
          </div>
        </div>
      </div>

      <InventoryWindow
        :isInventoryWindowVisible="isInventoryWindowVisible"
        @closeInventoryWindow="handleInventaryItemClick"
      >
        <InventoryDetails
          v-if="inventoryDetails"
          :id="inventoryDetails.id"
          :image="inventoryDetails.image"
          :description="inventoryDetails.description"
        />
      </InventoryWindow>

      <div class="inventory-block">
        <div class="inventory-block__content">
          <img :src="'../src/assets/images/pict3.png'" alt="pict3" />
        </div>
      </div>
    </div>
  </main>
</template>

<style scoped>
.main {
  padding: 32px;
}
.inventory {
  position: relative;
}
.inventory-scoreboard {
  margin-bottom: 24px;
  display: grid;
  grid-template-columns: 236px 535px;
  column-gap: 24px;
}
.inventory-scoreboard__account {
  padding: 18px 14px 24px 14px;

  display: grid;
  row-gap: 20px;

  border-radius: 12px;
  border: 1px solid #4d4d4d;
  background: #262626;
}

.inventory-scoreboard__info {
  text-align: center;
}

.inventory-scoreboard__action {
  display: grid;
  grid-template-columns: repeat(5, 1fr);
  border-radius: 12px;
  border: 1px solid #4d4d4d;
  background: #262626;
}

.inventory-scoreboard__item {
  position: relative;
  width: 105px;
  height: 100px;

  display: grid;
  align-items: center;
  justify-items: center;

  border: 1px solid #4d4d4d;
}

.inventory-scoreboard__item:hover {
  cursor: url('@/assets/images/pict8.png'), pointer;
  background: #2f2f2f;
}

.inventory-scoreboard__item:nth-child(1) {
  border-top-left-radius: 12px;
}

.inventory-scoreboard__item:nth-child(5) {
  border-top-right-radius: 12px;
}

.inventory-scoreboard__item:nth-child(21) {
  border-bottom-left-radius: 12px;
}

.inventory-scoreboard__item:nth-child(25) {
  border-bottom-right-radius: 12px;
}
.inventory-scoreboard__quantity {
  position: absolute;
  bottom: 0;
  right: 0;
  width: 20px;
  height: 20px;

  display: flex;
  align-items: center;
  justify-content: center;

  color: #fff;
  opacity: 0.4;
  text-align: center;
  font-size: 10px;
  font-style: normal;
  font-weight: 500;
  line-height: normal;

  border-radius: 6px 0px 0px 0px;
  border: 1px solid #4d4d4d;
  background: #262626;
}
.inventory-block {
  position: relative;
  padding: 18px;

  border-radius: 12px;
  border: 1px solid #4d4d4d;
  background: #262626;
}
.inventory-block::before {
  content: '';
  position: absolute;
  top: 14%;
  right: 1%;
  width: 24px;
  height: 24px;

  background-image: url('@/assets/images/pict4.png');
}

@media (max-width: 858.98px) {
  .inventory-scoreboard {
    grid-template-columns: 100%;
    row-gap: 24px;
  }

  .inventory-scoreboard__photo {
    text-align: center;
  }

  .inventory-block__content img {
    max-width: 95%;
  }

  .inventory-scoreboard__item {
    width: auto;
  }
}

@media (max-width: 575.98px) {
  .inventory-scoreboard__item:nth-child(3) {
    border-bottom-left-radius: 12px;
  }
  .inventory-scoreboard__item:nth-child(5) {
    border-top-right-radius: 0;
  }

  .inventory-scoreboard__item:nth-child(21) {
    border-bottom-left-radius: 0;
  }

  .inventory-scoreboard__item:nth-child(25) {
    border: none;
  }

  .inventory-scoreboard__action {
    grid-template-columns: repeat(3, 1fr);
  }
}
</style>
