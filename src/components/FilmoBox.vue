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
<MovieNavBar />
    <div class="container bg-black bg-gradient p-4 rounded shadow-lg">
      <div class="row">
        
        <MovieSideBar
        @select="selectedCategoryToFilter" />
        <MovieInteractiveList
        :selectedCategory />
      </div>
       
  </div>
</template>

<style scoped>
</style>