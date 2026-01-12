<script setup lang="ts">
type Item = {
  id: number
  name: string
}

defineProps<{
  item: Item
  selected?: boolean
}>()

const emit = defineEmits<{
  (e: 'click', item: Item): void
}>()
</script>

<template>
  <button
    class="item-card"
    :class="{ selected }"
    type="button"
    @click="emit('click', item)"
  >
    {{ item.name }}
  </button>
</template>

<style scoped>
.item-card {
  min-width: 104px;
  height: 44px;
  padding: 0 12px;

  background: #ffffff;
  border: 1px solid rgba(11, 15, 25, 0.14);
  border-radius: 12px;

  font-size: 13px;
  font-weight: 600;
  color: #0b0f19;

  cursor: pointer;

  transition:
    transform 120ms ease,
    box-shadow 160ms ease,
    border-color 160ms ease,
    background 160ms ease;
}

.item-card:hover {
  transform: translateY(-1px);
  box-shadow: 0 10px 22px rgba(11, 15, 25, 0.08);
}

.item-card:active {
  transform: translateY(0);
}

.item-card.selected {
  background: linear-gradient(
    180deg,
    #ffffff 0%,
    #f3f5fb 100%
  );

  border-color: rgba(11, 15, 25, 0.45);

  box-shadow:
    0 14px 28px rgba(11, 15, 25, 0.14),
    inset 0 0 0 1px rgba(11, 15, 25, 0.08);

  color: #0b0f19;
}

.item-card.selected:hover {
  box-shadow:
    0 18px 34px rgba(11, 15, 25, 0.18),
    inset 0 0 0 1px rgba(11, 15, 25, 0.10);
}
</style>
