<script lang="ts" setup>
import { getLimitedPosts, Post } from '@/api/api';
import { onBeforeMount, ref, watch } from 'vue';
import Pagination from "@/components/Pagination.vue";
import { useRoute } from "vue-router";

const route = useRoute();
const posts = ref<Post[]>();
const total = ref(0);
const max = ref(0);
const page = ref(1);

onBeforeMount(() => loadPosts());
watch(() => route.query, () => loadPosts())

async function loadPosts() {
  let skip = 0;
  const step = 5;

  if (route.query?.page) {
    const value = (+route.query?.page - 1) * step;
    skip = Math.min(value, total.value);
    page.value = Math.ceil(skip / step) + 1;
  }

  const response = (await getLimitedPosts(step, skip));
  console.log(response)
  total.value = response.total
  posts.value = response.posts
  max.value = Math.ceil(total.value / step)
}
</script>

<template>
  <h1>Posts list</h1>
  <div class="pagination-wrapper">
    <Pagination v-if="posts" :page="page" :max="max" />
  </div>
  <RouterLink class="post" v-for="(post, index) of posts" :to="{ name: 'post', params: { postId: post.id } }"
    :key="index">
    <div class="post__id">#{{ post.id }}</div>
    <div class="post__title">{{ post.title }}</div>
    <div>{{ post.body }}</div>
  </RouterLink>
  <div class="pagination-wrapper">
    <Pagination v-if="posts" :page="page" :max="max" />
  </div>
</template>

<style scoped>
.post {
  display: grid;
  grid-template-columns: min-content 1fr;
  grid-template-rows: repeat(2, auto);
  gap: 8px;

  &+& {
    margin-top: 12px;
    padding-top: 12px;
    border-top: 1px solid #ccc;
  }

  &__id {
    grid-row: -1/1;
  }

  &__title {
    font-weight: 700;
  }
}

.pagination-wrapper {
  display: flex;
  justify-content: center;
  margin: 5px;
}
</style>
