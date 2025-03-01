<script setup lang="ts">
import  { movieCategories } from '../scripts/movie';
import  { type MovieCategory } from '../scripts/movie';
import { ref} from 'vue';
import { defineEmits } from 'vue';

const selectedCategory = ref<MovieCategory>("Toutes");

const emit = defineEmits<{
(e: 'select', category: MovieCategory): void;
}>();

const handleSelect = (category: MovieCategory) => {
selectedCategory.value = category;
emit('select', category);
};
</script>

<template>
    <div class="col-md-3">
      <h4 class="mb-3">Catégories de films</h4>
      <ul class="list-group">
        <li v-for="category in movieCategories"
         :key="category"
         class="list-group-item"
         :class="{ active: category === selectedCategory }"
         @click="handleSelect(category)">
            {{ category }}
        </li>
      </ul>
    </div>
</template>

<style scoped>
</style>