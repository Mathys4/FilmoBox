<script setup lang="ts">
import { movies } from "../scripts/baseMovies.ts";
import { type DetailedMovie, type Movie, type MovieCategory } from "../scripts/movie.ts";
import  MovieCard  from "./MovieCard.vue";
import  MovieDetailedCard  from "./MovieDetailedCard.vue";
import { ref, watch } from 'vue';
import MovieForm from "./MovieForm.vue";
import { MovieCategoryEnum } from "../scripts/movie.ts";
import MovieModifyPreviewAndForm from "./MovieModifyPreviewAndForm.vue";
import MovieConfirmationDelete from "./MovieConfirmationDelete.vue";
import MovieDuplicate from "./MovieDuplicate.vue";

const props = defineProps<{
    selectedCategory: MovieCategory;
}>();


const showUploadForm = ref(false);

const showModifyForm = ref(false);

const showDuplicateForm = ref(false);

const showDetailedView = ref(false);

const showDeleteConfirmationMessage = ref(false);

const initialMovieList = ref<(Movie & Partial<DetailedMovie>)[]>(movies);
const moviesInteractiveList = ref<(Movie & Partial<DetailedMovie>)[]>(initialMovieList.value);

const inputRecherche = ref('');
const filterStock = ref('all');

const addNewMovie = (movie: Movie) => {
    //Ajjoute un id au nouveau film créer automatiquement
    movie.id = initialMovieList.value.length + 1;
    //Ajoute le nouveau film à la liste des films
    initialMovieList.value.push(movie);
    //Cache le formulaire après que le film soit ajouté
    showUploadForm.value = false;
    showDuplicateForm.value = false;
    researchInMovieList();
}

const modifyMovie = (movieToModify: Movie) => {
    //Trouve le film à modifier avec le id
    const index = initialMovieList.value.findIndex((movie) => movie.id === movieToModify.id);
    //Modifie le film pour les nouvelles valeurs du formulaire
    initialMovieList.value[index] = movieToModify;
    //Cache le formulaire après que le film soit modifier
    showModifyForm.value = false;
    researchInMovieList();
}

const selectedMovieToModify = ref<Movie>(
    {
        id: 0,
        name: "",
        description: "",
        category: MovieCategoryEnum.Toutes,
        stock: 0,
        imageUrl:""
    }
);

const selectedMovieToDelete = ref<Movie>(
    {
        id: 0,
        name: "",
        description: "",
        category: MovieCategoryEnum.Toutes,
        stock: 0,
        imageUrl:""
    }
);

const selectedMovieToDuplicate = ref<Movie>(
    {
        id: 0,
        name: "",
        description: "",
        category: MovieCategoryEnum.Toutes,
        stock: 0,
        imageUrl:""
    }
);

const selectedMovieToViewDetail = ref<DetailedMovie>(
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

const movieToViewDetails = (movieToView: Movie) => {
    showDetailedView.value = true;

    // Fix the findIndex call - comparing the correct IDs
    const index = initialMovieList.value.findIndex((m) => m.id === movieToView.id);
        const foundMovie = initialMovieList.value[index];
        selectedMovieToViewDetail.value.id = foundMovie.id;
        selectedMovieToViewDetail.value.name = foundMovie.name;
        selectedMovieToViewDetail.value.description = foundMovie.description;
        selectedMovieToViewDetail.value.category = foundMovie.category;
        selectedMovieToViewDetail.value.stock = foundMovie.stock;
        selectedMovieToViewDetail.value.imageUrl = foundMovie.imageUrl;
        selectedMovieToViewDetail.value.producer = foundMovie.producer || "";
        selectedMovieToViewDetail.value.releaseDate = foundMovie.releaseDate || "";
        selectedMovieToViewDetail.value.duration = foundMovie.duration || 0;
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
const deleteMovie = () => {
    //Trouve le film à supprimer avec le id
    const index = initialMovieList.value.findIndex((movie) => movie.id === selectedMovieToDelete.value.id);
    //Supprime le film de la liste des films
    initialMovieList.value.splice(index, 1);
    showDeleteConfirmationMessage.value = false;
}

const researchInMovieList = () => {
    moviesInteractiveList.value = initialMovieList.value;
    if (inputRecherche.value.trim() === '') {
        moviesInteractiveList.value = initialMovieList.value;
    } else {
        // Filtre les films avec le nom recherché
        moviesInteractiveList.value = initialMovieList.value.filter((movie) => 
            movie.name.toLowerCase().includes(inputRecherche.value.toLowerCase())
        );
    }
};
watch(
  () => props.selectedCategory, () => { sortMoviesByCategory() }
);
const sortMoviesByCategory = () => {
    moviesInteractiveList.value = initialMovieList.value;
    if(props.selectedCategory === 'Toutes') {
        moviesInteractiveList.value = initialMovieList.value;
    }
    else {
        // Filtre les films avec la catégorie sélectionnée
         moviesInteractiveList.value = initialMovieList.value.filter((movie) => movie.category === props.selectedCategory);
    }
};

const researchInMovieListWithStock = () => {
    moviesInteractiveList.value = initialMovieList.value;
    if (filterStock.value === 'Tous') {
        moviesInteractiveList.value = initialMovieList.value;
    } else if (filterStock.value === 'Dispo') {
        // Filtre les films avec le stock disponible
        moviesInteractiveList.value = initialMovieList.value.filter((movie) => 
            movie.stock > 0
        );
    } else if (filterStock.value === 'Aucun') {
        // Filtre les films avec le stock épuisé
        moviesInteractiveList.value = initialMovieList.value.filter((movie) => 
            movie.stock === 0
        );
    }
};

const isAnyMovieOutOfStock = () => {
    // Filtre les films avec le stock disponible
    moviesInteractiveList.value = initialMovieList.value.filter((movie) => movie.stock === 0);
    if(moviesInteractiveList.value.length !== 0) {
        alert('Ces films sont hors stocks : ' + moviesInteractiveList.value.map((movie) => movie.name).join(', '));
    }
    moviesInteractiveList.value = initialMovieList.value;
};

const setFiltreStock = (filter: 'Tous' | 'Dispo' | 'Aucun') => {
    filterStock.value = filter;
    researchInMovieListWithStock();
};
isAnyMovieOutOfStock();

const exportMovies = () => {
  // exportToCSV(moviesInteractiveList.value, 'movies.csv');
};
</script>

<template>
    
    <div class="col">
        <h4 class="mb-3">Liste des films</h4>
        <div class="row">
            <div class="btn-group">
                <button  v-if="!showUploadForm" @click="showUploadForm = true" type="button" class="btn btn-primary">Ajouter un nouveau film</button>
                <button  v-if="showUploadForm" @click="showUploadForm = false" type="button" class="btn btn-danger">Fermer le formulaire</button>
            </div>
            <div v-if="showUploadForm" class="col">
                <MovieForm
                @submit="addNewMovie" />
            </div>
        </div>
        <div class="row">
            <div class="btn-group">
                <button type="button" class="btn btn-secondary" @click="setFiltreStock('Tous')">Tous</button>
                <button type="button" class="btn btn-success" @click="setFiltreStock('Dispo')">Disponibles</button>
                <button type="button" class="btn btn-danger" @click="setFiltreStock('Aucun')">Hors Stock</button>
                <button class="btn btn-info" @click="exportMovies"><i class="bi bi-download"></i> Export to CSV</button>
            </div>
            <div class="input-group mb-3">
                <input v-model="inputRecherche" type="text" class="form-control" placeholder="Rechercher un film" aria-label="Rechercher un film">
                <button @click="researchInMovieList" class="btn btn-outline-secondary" type="button" id="researchButton">Rechercher</button>
            </div>
            <div v-if="showDeleteConfirmationMessage" class="container mt-4">
                <MovieConfirmationDelete
                    @confirm="handleConfirm"
                    @cancel="handleCancel" />
            </div>
            <div v-if="!showModifyForm && !showDeleteConfirmationMessage && !showDuplicateForm && !showDetailedView" class="container">
                <div class="row row-cols-1 row-cols-md-2 row-cols-lg-3 g-4">
                    <div v-for="movie in moviesInteractiveList" :key="movie.id" class="col">
                        <MovieCard 
                        :movie="movie"
                        @modify="movieToModify"
                        @delete="movieToDelete"
                        @duplicate="movieToDuplicate"
                        @view="movieToViewDetails"
                        />
                    </div>
                </div>
            </div>
            <div v-if="showDetailedView" class="container py-4">
                <MovieDetailedCard
                :movie="selectedMovieToViewDetail"
                @back="showDetailedView = false"
                @modify="movieToModify"
                @delete="movieToDelete"
                @duplicate="movieToDuplicate"
                @view="movieToViewDetails" />
            </div>
            <div v-if="showModifyForm" class="container py-4">
                <MovieModifyPreviewAndForm
                :selectedMovie = selectedMovieToModify
                @submit="modifyMovie" />
            </div>
            <div v-if="showDuplicateForm" class="container py-4">
                <MovieDuplicate
                :selectedMovie = selectedMovieToDuplicate
                @submit="addNewMovie" />
            </div>
        </div>
    </div>
</template>

<style scoped>
</style>