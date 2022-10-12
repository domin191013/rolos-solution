<script lang="ts" setup>
import { ref, watch } from "vue";
import PaginationItem from "@/components/PaginationItem.vue";

const props = defineProps({
  page: {
    type: Number,
    required: true,
  },
  max: {
    type: Number,
    required: true,
  },
});

</script>
<template>
  <div class="pagination">
    <PaginationItem v-if="page > 1" :page="1" />
    <PaginationItem v-if="page > 2" :page="2" />

    <template v-if="page - 3 > 2">
      <PaginationItem v-if="page - 5 === 1" :page="3" />
      <div v-else>...</div>
    </template>

    <PaginationItem v-if="page - 2 > 2" :page="page - 2" />
    <PaginationItem v-if="page - 1 > 2" :page="page - 1" />
    <PaginationItem :page="page" selected />
    <PaginationItem v-if="page + 1 < max - 1" :page="page + 1" />
    <PaginationItem v-if="page + 2 < max - 1" :page="page + 2" />

    <template v-if="page + 3 < max - 1">
      <PaginationItem v-if="page + 5 === max" :page="page + 3" />
      <div v-else>...</div>
    </template>

    <PaginationItem v-if="page < max - 1" :page="max - 1" />
    <PaginationItem v-if="page < max" :page="max" />
  </div>
</template>
<style scoped>
.pagination {
  display: flex;
  gap: 15px;
}
</style>
