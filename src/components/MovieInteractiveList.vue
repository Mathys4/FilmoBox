<script setup lang="ts">
import { movies } from "../scripts/baseMovies.ts";
import { type Movie, type MovieCategory, type StockFilter } from "../scripts/movie.ts";
import  MovieCard  from "./MovieCard.vue";
import  MovieDetailedCard  from "./MovieDetailedCard.vue";
import { ref, watch, onMounted } from 'vue';
import MovieUploadForm from "./MovieUploadForm.vue";
import { MovieCategoryEnum } from "../scripts/movie.ts";
import MovieModifyForm from "./MovieModifyForm.vue";
import MovieConfirmationDelete from "./MovieConfirmationDelete.vue";
import MovieDuplicateForm from "./MovieDuplicateForm.vue";
import MovieOutStockAlert from "./MovieOutStockAlert.vue";
import MovieStockFilter from "./MovieStockFilter.vue";
import ExportButton from "./ExportButton.vue";
import MovieSearch from "./MovieSearch.vue";

const props = defineProps<{
    selectedCategory: MovieCategory;
}>();

onMounted(() => {
    isAnyMovieOutOfStock();
})

const isAnyMovieOutOfStock = () => {
    outOfStockMovies.value = moviesInteractiveList.value.filter((movie) => movie.stock === 0);
    if(outOfStockMovies.value.length !== 0) {
        showAlertOutOfStock.value = true;
    }
    else {
        showAlertOutOfStock.value = false;  
    }
}

watch(() => props.selectedCategory, () => {
    sortMoviesByCategory();
})

const sortMoviesByCategory = () => {
    moviesInteractiveList.value = initialMovieList.value;
    if(props.selectedCategory === 'Toutes') {
        moviesInteractiveList.value = initialMovieList.value;
    }
    else {
         moviesInteractiveList.value = initialMovieList.value.filter((movie) => movie.category === props.selectedCategory);
    }
}


const showModifyForm = ref(false);
const showDuplicateForm = ref(false);
const showDetailedView = ref(false);
const showDeleteConfirmationMessage = ref(false);
const showAlertOutOfStock = ref(false);


const initialMovieList = ref<(Movie)[]>(movies);
const moviesInteractiveList = ref<(Movie)[]>(initialMovieList.value);
const outOfStockMovies = ref<(Movie)[]>([]);

const addNewMovie = (movie: Movie) => {
    movie.id = initialMovieList.value.length + 1;
    initialMovieList.value.push(movie);
}

const duplicateMovie = (movie: Movie) => {
    movie.id = initialMovieList.value.length + 1;
    initialMovieList.value.push(movie);
    showDuplicateForm.value = false;
}
const modifyMovie = (movieToModify: Movie) => {
    const index = initialMovieList.value.findIndex((movie) => movie.id === movieToModify.id);
    initialMovieList.value[index] = movieToModify;
    showModifyForm.value = false;
}

const deleteMovie = () => {
    const index = initialMovieList.value.findIndex((movie) => movie.id === selectedMovieToDelete.value.id);
    initialMovieList.value.splice(index, 1);
    showDeleteConfirmationMessage.value = false;
}
const selectedMovieToModify = ref<Movie>(
    {
        id : 0,
        name : "",
        producer : "",
        releaseDate : "",
        duration : 0,
        category : MovieCategoryEnum.Toutes,
        description : "",
        stock : 0,
        imageUrl : ""
    }
);

const selectedMovieToDelete = ref<Movie>(
    {
        id : 0,
        name : "",
        producer : "",
        releaseDate : "",
        duration : 0,
        category : MovieCategoryEnum.Toutes,
        description : "",
        stock : 0,
        imageUrl : ""
    }
);

const selectedMovieToDuplicate = ref<Movie>(
    {
        id : 0,
        name : "",
        producer : "",
        releaseDate : "",
        duration : 0,
        category : MovieCategoryEnum.Toutes,
        description : "",
        stock : 0,
        imageUrl : ""
    }
);

const selectedMovieToViewDetail = ref<Movie>(
    {
        id : 0,
        name : "",
        producer : "",
        releaseDate : "",
        duration : 0,
        category : MovieCategoryEnum.Toutes,
        description : "",
        stock : 0,
        imageUrl : ""
    }
);

const movieToViewDetails = (movie: Movie) => {
    selectedMovieToViewDetail.value = movie;
    showDetailedView.value = true;
};

const movieToModify = (movie: Movie) => {
    selectedMovieToModify.value = movie;
    showModifyForm.value = true;
}

const movieToDuplicate = (movie: Movie) => {
    selectedMovieToDuplicate.value = movie;
    showDuplicateForm.value = true;
}

const movieToDelete = (movie: Movie) => {
    showDeleteConfirmationMessage.value = true;
    selectedMovieToDelete.value = movie;
}

const handleConfirm = () => {
    deleteMovie();
    showDeleteConfirmationMessage.value = false
}

const handleCancel = () => {
    showDeleteConfirmationMessage.value = false
}


const researchInMovieList = (inputQuery: string) => {
    moviesInteractiveList.value = initialMovieList.value;
    if (inputQuery.trim() === '') {
        moviesInteractiveList.value = initialMovieList.value;
    } else {
        // Filtre les films avec le nom recherché
        moviesInteractiveList.value = moviesInteractiveList.value.filter((movie) => 
            movie.name.toLowerCase().includes(inputQuery.toLowerCase())
        );
    }
};

const researchInMovieListWithStock = (filter: StockFilter) => {
    moviesInteractiveList.value = initialMovieList.value;
    if (filter === 'Dispo') {
        // Filtre les films avec le stock disponible
        moviesInteractiveList.value = initialMovieList.value.filter((movie) => 
            movie.stock > 0
        );
    }
    if (filter === 'Aucun') {
        // Filtre les films avec le stock épuisé
        moviesInteractiveList.value = initialMovieList.value.filter((movie) => 
            movie.stock === 0
        );
    }
    if (filter === 'Tous') {
        moviesInteractiveList.value = initialMovieList.value;
    }
};

</script>

<template>
    <div class="container mt-4">
        <div v-if="showAlertOutOfStock">
            <MovieOutStockAlert
                :outOfStockMovies="outOfStockMovies"
                @close="showAlertOutOfStock=false"/>
        </div>
      <div class="d-flex justify-content-between align-items-center mb-4">
        
        <h1 class="h1 text-dark">Liste des films</h1>
        <MovieUploadForm @submit="addNewMovie" />
      </div>

      <div class="d-flex flex-wrap gap-2 mb-4">
        <MovieStockFilter @changeFilter="researchInMovieListWithStock" />
        <ExportButton :movie-list-to-import="moviesInteractiveList" />
      </div>

      <MovieSearch @search="researchInMovieList" />
    </div>
        <div v-if="showDeleteConfirmationMessage" class="container mt-4">
            <MovieConfirmationDelete
                @confirm="handleConfirm"
                @cancel="handleCancel" />
        </div>
        <div v-if=" !showDeleteConfirmationMessage" class="container">
            <div class="row row-cols-1 row-cols-md-2 row-cols-lg-3 g-4">
                <div v-for="movie in moviesInteractiveList" :key="movie.id" class="col" >
                    <MovieCard 
                        :movie="movie"
                        @modify="movieToModify"
                        @delete="movieToDelete"
                        @duplicate="movieToDuplicate"
                        @click="movieToViewDetails(movie)"/>
                </div>
            </div>
        </div>
        <div v-if="showDetailedView" class="container py-4">
            <MovieDetailedCard
                :movie="selectedMovieToViewDetail"
                @back="showDetailedView = false" />
        </div>
        <div v-if="showModifyForm" class="container py-4">
            <MovieModifyForm
                :selectedMovie = selectedMovieToModify
                @submit="modifyMovie" />
        </div>
        <div v-if="showDuplicateForm" class="container py-4">
            <MovieDuplicateForm
                :selectedMovie = selectedMovieToDuplicate
                @submit="duplicateMovie" />
        </div>
</template>

<style scoped>
</style>