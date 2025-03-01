<script setup lang="ts"> 
import { type Movie  } from "../scripts/movie.ts";
import { defineProps, defineEmits } from 'vue';

const props = defineProps<{
    movie: Movie;
}>();

const openDetailView = () => {
    emit('view', props.movie);
};

const openModifyForm = () => {
    emit('modify', props.movie);
};

const deleteMovie = () => {
    emit('delete', props.movie);
};

const duplicateMovie = () => {
    emit('duplicate', props.movie);
};

const emit = defineEmits<{
    (event: 'view', movie: Movie): void
    (event: 'modify', movie: Movie): void
    (event: 'delete', movie: Movie): void
    (event: 'duplicate', movie: Movie): void
}>();


</script>

<template>
    <div class="card h-100 border-dark">
        <div class="card-body d-flex flex-column">
            <img v-bind:src="movie.imageUrl" alt="Poster du film " class="card-img-top">
            <h5 class="card-title">{{ movie.name }}</h5>
            <p class="card-text">{{ movie.description }}</p>
            <p class="card-text"><small class="text-muted">{{ movie.category }}</small></p>
            <p class="card-text">
                <span :class="{'text-success': movie.stock > 5,'text-warning': movie.stock > 0 && movie.stock <= 5,'text-danger': movie.stock === 0}">
                    Stock: {{ movie.stock }}
                    <i :class="{'bi bi-check-circle-fill': movie.stock > 5,'bi bi-exclamation-triangle-fill': movie.stock > 0 && movie.stock <= 5,'bi bi-x-circle-fill': movie.stock === 0}"></i>
                </span>
            </p>
            <div class="row mt-auto">
                <button class="col btn btn-info"
                @click="openDetailView">
                    <i class="bi bi-eye me-2"></i>Voir
                </button>
                <button class="col btn btn-primary"
                @click="openModifyForm">
                    <i class="bi bi-pencil-square me-1"></i>Modifier
                </button>
                <button class="col btn btn-danger"
                @click="deleteMovie">
                    <i class="bi bi-trash m-2"></i>Supprimer
                </button>
                <button class="col btn btn-warning"
                @click="duplicateMovie">
                    <i class="bi bi-back m-2"></i>Dupliquer
                </button>
            </div>
        </div>
    </div>
</template>

<style scoped>
</style>