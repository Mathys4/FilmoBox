<script setup lang="ts">
import { ref, defineEmits } from "vue";

const emit = defineEmits(["search"]);
const searchQuery = ref("");

const researchInMovieList = () => {
  emit("search", searchQuery.value);
};
</script>

<template>
    <div class="input-group mb-3">
        <div class="input-group-prepend">
            <span class="input-group-text" id="basic-addon1">
                <i class="bi bi-search text-gray-400"></i>
            </span>
        </div>
        <input
            v-model="searchQuery"
            @input="researchInMovieList"
            type="text"
            class="form-control"
            placeholder="Rechercher un film..."/>
    </div>

    <!--
    <div class="input-group mb-3">
    <input v-model="inputRecherche" type="text" class="form-control" placeholder="Rechercher un film" />
    <button @click="researchInMovieList" class="btn btn-outline-secondary">Rechercher</button>
  </div>-->
</template>

<style scoped>
</style>