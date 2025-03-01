<script setup lang="ts">
import { type DetailedMovie } from "../scripts/movie.ts";
import { defineProps, defineEmits } from 'vue';

const props = defineProps<{
    movie: DetailedMovie;
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

const toHomePage = () => {
    emit('back', props.movie);
};

const emit = defineEmits<{
    (event: 'view', movie: DetailedMovie): void
    (event: 'modify', movie: DetailedMovie): void
    (event: 'delete', movie: DetailedMovie): void
    (event: 'duplicate', movie: DetailedMovie): void
    (event: 'back', movie: DetailedMovie): void
}>();
</script>

<template>
    <div class="card h-100 border-dark">
        <div class="row g-0">
            <div class="col-md-4">
                <img v-bind:src="movie.imageUrl" alt="Poster du film" class="img-thumbnail img-fluid" width="300" height="200">
            </div>
            <div class="col-md-8">
                <div class="card-body d-flex flex-column">
                    <h5 class="card-title">{{ movie.name }}</h5>
                    <p class="card-text">{{ movie.description }}</p>
                    <div class="movie-details">
                        <p class="card-text"><small class="text-muted">Catégorie: {{ movie.category }}</small></p>
                        <p class="card-text"><small class="text-muted">Producteur: {{ movie.producer }}</small></p>
                        <p class="card-text"><small class="text-muted">Date de sortie: {{ movie.releaseDate }}</small></p>
                        <p class="card-text"><small class="text-muted">Durée: {{ movie.duration }} min</small></p>
                        <p class="card-text">
                            <span :class="{'text-success': movie.stock > 5,'text-warning': movie.stock > 0 && movie.stock <= 5,'text-danger': movie.stock === 0}">
                                Stock: {{ movie.stock }}
                                <i :class="{'bi bi-check-circle-fill': movie.stock > 5,'bi bi-exclamation-triangle-fill': movie.stock > 3 && movie.stock <= 7,'bi bi-x-circle-fill': movie.stock < 3}"></i>
                            </span>
                        </p>
                    </div>
                    <div class="row mt-auto">
                        <button class="col btn btn-info" @click="openDetailView">
                            <i class="bi bi-eye me-2"></i>Voir
                        </button>
                        <button class="col btn btn-primary" @click="openModifyForm">
                            <i class="bi bi-pencil-square me-1"></i>Modifier
                        </button>
                        <button class="col btn btn-danger" @click="deleteMovie">
                            <i class="bi bi-trash m-2"></i>Supprimer
                        </button>
                        <button class="col btn btn-warning" @click="duplicateMovie">
                            <i class="bi bi-back m-2"></i>Dupliquer
                        </button>
                    </div>
                    <button class="col btn btn-secondary mt-2" @click="toHomePage">
                        <i class="bi bi-house-fill m-2"></i>Revenir à la page d'accueil
                    </button>
                </div>
            </div>
        </div>
    </div>
</template>

<style scoped>
.movie-details {
    margin-bottom: 1rem;
}

.movie-details p {
    margin-bottom: 0.25rem;
}
</style>