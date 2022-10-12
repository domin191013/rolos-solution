<script lang="ts" setup>
defineProps({
  page: {
    type: Number,
  },
  isSelected: {
    type: Boolean,
  },
});
</script>
<template>
  <RouterLink class="pagination-item-wrapper" :class="isSelected && 'pagination-item-selected'"
    :to="{ name: 'posts', query: { page: page }}">
    {{ page }}
  </RouterLink>
</template>
<style>
.pagination-item-wrapper {
  cursor: pointer;
}

.pagination-item-selected {
  color: darkred;
}
</style>
