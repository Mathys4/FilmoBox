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
    <aside class="col-md-2 bg-white shadow-lg position-fixed h-100">
      <div class="p-3 border-bottom">
        <h4 class="fw-semibold text-dark">Catégories de films</h4>
      </div>
        <nav class="p-3">
          <ul class="list-unstyled">
            <li v-for="category in movieCategories"
              :key="category"
              @click="handleSelect(category)"
              :class="{'bg-primary-subtle text-primary': category === selectedCategory, 'text-dark': category !== selectedCategory}"
              class="d-flex align-items-center p-2 rounded">
              <span>{{ category }}</span>
            </li>
        </ul>
      </nav>
    </aside>
</template>

<style scoped>
</style>