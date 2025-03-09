<script setup lang="ts">
import MovieSideBar from './MovieSideBar.vue'
import MovieInteractiveList from './MovieInteractiveList.vue';
import MovieNavBar from './MovieNavBar.vue';
import  { type MovieCategory } from '../scripts/movie';
import { ref} from 'vue';
const selectedCategory = ref<MovieCategory>("Toutes");

const selectedCategoryToFilter = (category: MovieCategory) => {
    selectedCategory.value = category;
};
</script>

<template>
  <div class="row">
    <div class="col-md-2">
      <MovieSideBar @select="selectedCategoryToFilter" />
    </div>
    <div class="col-md-10">
      <main class="flex-grow-1 bg-secondary bg-opacity-10">
        <MovieNavBar />
        <MovieInteractiveList :selectedCategory />
      </main>
    </div>
  </div> 
</template>

<style scoped>
</style>