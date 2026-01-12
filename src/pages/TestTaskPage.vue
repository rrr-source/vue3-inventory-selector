<script setup lang="ts">
import { ref, computed } from 'vue'
import ItemCard from '@/components/ItemCard.vue'

type Item = {
  id: number
  name: string
}

const userItems: Item[] = [
  { id: 1, name: 'Shoes 1' },
  { id: 2, name: 'Shoes 2' },
  { id: 3, name: 'Shoes 3' },
  { id: 4, name: 'Shoes 4' },
  { id: 5, name: 'T-shirt 1' },
  { id: 6, name: 'T-shirt 2' },
  { id: 7, name: 'T-shirt 3' },
  { id: 8, name: 'T-shirt 4' },
]

const shopItems: Item[] = [
  { id: 11, name: 'Jacket 1' },
  { id: 12, name: 'Jacket 2' },
  { id: 13, name: 'Jacket 3' },
  { id: 14, name: 'Jacket 4' },
  { id: 15, name: 'Hoodie 1' },
  { id: 16, name: 'Hoodie 2' },
  { id: 17, name: 'Hoodie 3' },
  { id: 18, name: 'Hoodie 4' },
]


const selectedUserItems = ref<Item[]>([])
const selectedShopItem = ref<Item | null>(null)

const selectedCount = computed(() => selectedUserItems.value.length)
const totalCount = computed(() => userItems.length)

const isUserSelected = (id: number) =>
  selectedUserItems.value.some(i => i.id === id)

const isShopSelected = (id: number) =>
  selectedShopItem.value?.id === id

function toggleUserItem(item: Item) {
  const index = selectedUserItems.value.findIndex(i => i.id === item.id)

  if (index !== -1) {
    selectedUserItems.value.splice(index, 1)
    return
  }

  if (selectedUserItems.value.length >= 6) return

  selectedUserItems.value.push(item)
}

function selectShopItem(item: Item) {
  selectedShopItem.value = item
}
</script>

<template>
  <div class="page">
    <header class="header">
      <h1>Goods Selector</h1>
      <p>Select up to 6 user items and 1 shop item</p>
    </header>

    <!-- TOP -->
    <div class="top">
      <!-- top-left -->
      <section class="panel small">
        <div class="panel-head">
          <span>Selected user items</span>
          <span class="counter">{{ selectedCount }} / {{ totalCount }}</span>
        </div>

        <div class="items">
          <ItemCard
            v-for="item in selectedUserItems"
            :key="item.id"
            :item="item"
            selected
            @click="toggleUserItem"
          />
        </div>

        <div v-if="!selectedUserItems.length" class="hint">
          Choose items from the list below (max 6)
        </div>
      </section>

      <!-- top-right -->
      <section class="panel small center">
        <div class="panel-head center">Selected shop item</div>

        <div
          class="selected-box"
          :class="{ empty: !selectedShopItem }"
        >
          {{ selectedShopItem?.name ?? 'SELECTED ITEM' }}
          <span v-if="!selectedShopItem">
            Select one item from the shop list
          </span>
        </div>
      </section>
    </div>

    <!-- BOTTOM -->
    <div class="bottom">
      <!-- bottom-left -->
      <section class="panel big">
        <div class="panel-head">
          <span>Your items</span>
          <span class="meta">Click to select / unselect</span>
        </div>

        <div class="items">
          <ItemCard
            v-for="item in userItems"
            :key="item.id"
            :item="item"
            :selected="isUserSelected(item.id)"
            @click="toggleUserItem"
          />
        </div>
      </section>

      <!-- bottom-right -->
      <section class="panel big">
        <div class="panel-head">
          <span>Shop items</span>
          <span class="meta">Only one allowed</span>
        </div>

        <div class="items">
          <ItemCard
            v-for="item in shopItems"
            :key="item.id"
            :item="item"
            :selected="isShopSelected(item.id)"
            @click="selectShopItem"
          />
        </div>
      </section>
    </div>
  </div>
</template>

<style scoped>
* {
  box-sizing: border-box;
}

.page {
  min-height: 100vh;
  padding: 28px;
  background: #f5f7fb;
  color: #0b0f19;
}

.header {
  max-width: 1100px;
  margin: 0 auto 20px;
}

.header h1 {
  margin: 0;
  font-size: 22px;
}

.header p {
  margin: 6px 0 0;
  font-size: 13px;
  opacity: 0.7;
}

.top,
.bottom {
  max-width: 1100px;
  margin: 0 auto;
  display: grid;
  gap: 24px;
}

.top {
  grid-template-columns: 1fr 300px;
  margin-bottom: 24px;
}

.bottom {
  grid-template-columns: 1fr 1fr;
}

.panel {
  background: #fff;
  border-radius: 14px;
  padding: 16px;
  box-shadow: 0 10px 30px rgba(11, 15, 25, 0.06);
}

.panel.small {
  min-height: 150px;
}

.panel.big {
  min-height: 300px;
}

.panel.center {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.panel-head {
  display: flex;
  justify-content: space-between;
  align-items: baseline;
  margin-bottom: 12px;
  font-size: 12px;
  font-weight: 700;
  text-transform: uppercase;
  letter-spacing: 0.08em;
  opacity: 0.75;
}

.panel-head.center {
  justify-content: center;
}

.meta {
  font-size: 11px;
  font-weight: 500;
  opacity: 0.6;
}

.items {
  display: flex;
  flex-wrap: wrap;
  gap: 12px;
}

.selected-box {
  width: 100%;
  height: 110px;
  border: 1px dashed rgba(11, 15, 25, 0.25);
  border-radius: 12px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 6px;
  font-weight: 700;
}

.selected-box.empty {
  opacity: 0.6;
}

.selected-box span {
  font-size: 12px;
  font-weight: 500;
  text-align: center;
}

.counter {
  font-size: 12px;
  font-weight: 500;
}

.hint {
  margin-top: 12px;
  font-size: 12px;
  opacity: 0.6;
}
</style>
