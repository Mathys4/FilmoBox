<script setup lang="ts">
import { ref, defineEmits } from 'vue';
import { MovieCategoryEnum, type DetailedMovie } from "../scripts/movie.ts";
import { type Movie  } from "../scripts/movie.ts";

const newMovie = ref<DetailedMovie>({
    id: 0,
    name: "",
    producer: "",
    releaseDate: "",
    duration: 0,
    category: MovieCategoryEnum.Toutes,
    description: "",
    stock: 0,
    imageUrl: ""
});

const emit = defineEmits<{
    (event: 'submit', movie: Movie): void
}>();

function verifMovie() {
    if (newMovie.value.name === ""
        || newMovie.value.description === ""
        || newMovie.value.category === MovieCategoryEnum.Toutes 
        || newMovie.value.stock === 0) {
        return false;
    }
    return true;
}

const submitNewMovie = () => {
  //Vérifie si le movie est bien valide
    if(verifMovie() === true) {
        //emit le nouveau film ajouté
        emit('submit', newMovie.value);
        //Reinitialise le formulaire pour ajouter un nouveau film
        newMovie.value = {
            id: 0,
            name: "",
            producer: "",
            releaseDate: "",
            duration: 0,
            category: MovieCategoryEnum.Toutes,
            description: "",
            stock: 0,
            imageUrl: ""
        };
    }
    else 
    {
        alert("Veuillez remplir tous les champs. Le film est invalide à l'ajout.");
    }
};
</script>

<template>
    <form>
        <div class="row">
            <div class="col-md-6 mb-3">
                <label for="movieName" class="form-label">Nom du film</label>
                <input type="text" class="form-control" id="movieName" placeholder="Entrez le nom du film"
                v-model="newMovie.name">
            </div>
            <div class="col-md-6 mb-3">
                <label for="movieProducer" class="form-label">Producteur</label>
                <input type="text" class="form-control" id="movieProducer" placeholder="Entrez le nom du producteur"
                v-model="newMovie.producer">
            </div>
        </div>
        <div class="row">
            <div class="col-md-6 mb-3">
                <label for="movieReleaseDate" class="form-label">Date de sortie</label>
                <input type="text" class="form-control" id="movieReleaseDate" placeholder="Entrez la date de sortie"
                v-model="newMovie.releaseDate">
            </div>
            <div class="col-md-6 mb-3">
                <label for="movieDuration" class="form-label">Durée (minutes)</label>
                <input type="number" class="form-control" id="movieDuration" placeholder="Entrez la durée du film"
                v-model.number="newMovie.duration">
            </div>
        </div>
        <div class="row">
            <div class="col-md-6 mb-3">
                <label for="movieDescription" class="form-label">Description</label>
                <input type="text" class="form-control" id="movieDescription" placeholder="Entrez la description du film"
                v-model="newMovie.description">
            </div>
            <div class="col-md-6 mb-3">
                <label for="movieCategory" class="form-label">Catégorie</label>
                <select class="form-select" id="movieCategory" v-model="newMovie.category">
                    <option value="" disabled>Sélectionnez une catégorie</option>
                    <option v-for="category in MovieCategoryEnum" 
                        :key="category" 
                        :value="category">
                        {{ category }}
                    </option>
                </select>
            </div>
        </div>
        <div class="row">
            <div class="col-md-6 mb-3">
                <label for="movieStock" class="form-label">Stock</label>
                <input type="number" class="form-control" id="movieStock" placeholder="Entrez le stock du film"
                v-model.number="newMovie.stock">
            </div>
            <div class="col-md-6 mb-3">
                <label for="movieImageUrl" class="form-label">URL de l'image</label>
                <input type="text" class="form-control" id="movieImageUrl" placeholder="Entrez l'URL de l'image"
                v-model="newMovie.imageUrl">
            </div>
        </div>
        <button type="button" class="btn btn-primary" @click="submitNewMovie">
            Ajouter
        </button>
    </form>
</template>

<style scoped>
</style>