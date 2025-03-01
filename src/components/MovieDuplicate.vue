<script setup lang="ts">
import { ref, defineEmits, defineProps } from 'vue';
import { MovieCategoryEnum } from "../scripts/movie.ts";
import  MovieCard  from "./MovieCard.vue";
import { type Movie  } from "../scripts/movie.ts";


const props = defineProps<{
    selectedMovie: Movie;
}>();

const selectedMovie = ref<Movie>(
    {
        id: props.selectedMovie.id,
        name: props.selectedMovie.name,
        description: props.selectedMovie.description,
        category: props.selectedMovie.category,
        stock: props.selectedMovie.stock,
        imageUrl: props.selectedMovie.imageUrl
    }
);

function verifMovie() {
    if (selectedMovie.value.name === ""
        || selectedMovie.value.description === ""
        || selectedMovie.value.category === MovieCategoryEnum.Toutes 
        || selectedMovie.value.stock === 0) {
        return false;
    }
    return true;
}
const resetMovie = () => {
    selectedMovie.value = {
        id: 0,
        name: "",
        description: "",
        category: MovieCategoryEnum.Toutes,
        stock: 0,
        imageUrl:""
    };
};

const emit = defineEmits<{
    (event: 'submit', movie: Movie): void
}>();

const submitNewModifiedMovie = () => {
  //Vérifie si le movie est bien valide
    if(verifMovie() === true) {
        //emit le nouveau film modifier
        emit('submit', selectedMovie.value);
        //Reinitialise le formulaire pour les prochain film
        resetMovie();
    }
    else 
    {
        alert("Veuillez remplir tous les champs. Le film est invalide à la modification");
    }
};
const abortModifying = () => {
  //Vérifie si le movie est bien valide
    if(verifMovie() === true) {
        //emit le film intial pour ne pas le changer
        emit('submit', props.selectedMovie);
        //Reinitialise le formulaire pour les prochain film
        resetMovie();
    }
    else 
    {
        alert("Veuillez remplir tous les champs. Le film est invalide à la modification");
    }
};
</script>

<template>
    <div class="d-flex">
        <div class="me-3">
            <div class="card-header bg-dark text-white rounded-2">
                <h5 class="mb-0">Aperçu du film</h5>
            </div>
            <MovieCard
            :movie="selectedMovie" />
        </div>
        <div class="flex-grow-1">
            <div class="card h-100 border-dark shadow-sm">
                <form>
                    <h3>Dupliquer le film</h3>
                    <div class="mb-3">
                        <label for="movieName" class="form-label">Nom du film</label>
                        <input type="text" class="form-control" id="movieName" placeholder="Entrez le nom du film à dupliquer"
                        v-model="selectedMovie.name">
                    </div>
                    <div class="mb-3">
                        <label for="movieDescription" class="form-label">Description</label>
                        <input type="text" class="form-control" id="movieDescription" placeholder="Entrez le description du film à dupliquer"
                        v-model="selectedMovie.description">
                    </div>
                    <div class="mb-3">
                        <label for="movieCategory" class="form-label">Catégorie</label>
                        <select class="form-select" id="movieCategory" v-model="selectedMovie.category">
                            <option value="" disabled>Sélectionnez une catégorie</option>
                            <option v-for="category in MovieCategoryEnum" 
                                :key="category" 
                                :value="category">
                                {{ category }}
                            </option>
                        </select>
                    </div>
                    <div class="mb-3">
                        <label for="movieStock" class="form-label">Stock</label>
                        <input type="text" class="form-control" id="movieStock" placeholder="Entrez le stock du film à dupliquer"
                        v-model.number="selectedMovie.stock">
                    </div>
                    <button  type="button" class="btn btn-success"
                        @click="submitNewModifiedMovie">
                        Enregistrer les modifications
                    </button>
                    <button type="button" class="btn btn-secondary ms-2"
                        @click="abortModifying">
                        Annuler
                    </button>
                </form>
            </div>
        </div>
    </div>
</template>

<style scoped>
</style>