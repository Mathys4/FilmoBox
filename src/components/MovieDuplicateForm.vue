<script setup lang="ts">
import { ref, defineEmits, defineProps } from 'vue';
import { MovieCategoryEnum } from "../scripts/movie.ts";
import { type Movie  } from "../scripts/movie.ts";


const props = defineProps<{
    selectedMovie: Movie;
}>();

const selectedMovie = ref<Movie>(
    {
        id: props.selectedMovie.id,
        name: props.selectedMovie.name,
        producer: props.selectedMovie.producer,
        releaseDate: props.selectedMovie.releaseDate,
        duration: props.selectedMovie.duration,
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
        id : 0,
        name : "",
        producer : "",
        releaseDate : "",
        duration : 0,
        category : MovieCategoryEnum.Toutes,
        description : "",
        stock : 0,
        imageUrl : ""
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
    <div class="modal-backdrop fade show"></div>
        <div class="modal fade show d-block">
            <div class="modal-dialog modal-dialog-centered">
                <div class="modal-content">
                    <div class="modal-header">
                        <h5 class="modal-title">Dupliquer</h5>
                    </div>
                    <div class="modal-body">
                        <form>
                            <div class="form-group mb-3">
                                <label for="movieName" class="form-label">Titre</label>
                                <input 
                                    v-model="selectedMovie.name" 
                                    type="text" 
                                    class="form-control" 
                                    id="movieName" />
                            </div>
                            <div class="form-group mb-3">
                                <label for="movieProducer" class="form-label">Directeur</label>
                                <input 
                                    v-model="selectedMovie.producer" 
                                    type="text" 
                                    class="form-control" 
                                    id="movieProducer" />
                            </div>
                            <div class="form-group mb-3">
                                <label for="movieReleaseDate" class="form-label">Date de sortie</label>
                                <input 
                                    v-model="selectedMovie.releaseDate" 
                                    type="date" 
                                    class="form-control" 
                                    id="movieReleaseDate" />
                            </div>
                            <div class="form-group mb-3">
                                <label for="movieDuration" class="form-label">Durée (minutes)</label>
                                <input 
                                    v-model.number="selectedMovie.duration" 
                                    type="number" 
                                    class="form-control" 
                                    id="movieDuration" />
                            </div>
                            <div class="form-group mb-3">
                                <label for="movieCategory" class="form-label">Catégorie</label>
                                <select class="form-select" id="movieCategory" v-model="selectedMovie.category" >
                                    <option value="" disabled>Sélectionnez une catégorie</option>
                                    <option v-for="category in MovieCategoryEnum" 
                                        :key="category" 
                                        :value="category">
                                        {{ category }}
                                    </option>
                                </select>
                            </div>
                            <div class="form-group mb-3">
                                <label for="movieStock" class="form-label">Stock</label>
                                <input 
                                    v-model.number="selectedMovie.stock" 
                                    type="number" 
                                    class="form-control" 
                                    id="movieStock" />
                            </div>
                            <div class="form-group mb-3">
                                <label for="movieImageUrl" class="form-label">URL de l'image</label>
                                <input 
                                    v-model="selectedMovie.imageUrl" 
                                    type="text" 
                                    class="form-control" 
                                    id="movieImageUrl"  />
                            </div>
                            <div class="form-group mb-3">
                                <label for="movieDescription" class="form-label">Description</label>
                                <textarea 
                                    v-model="selectedMovie.description" 
                                    class="form-control" 
                                    id="movieDescription" 
                                    rows="3" >
                                </textarea>
                            </div>
                            <div class="d-flex justify-content-end gap-4">
                                <button type="button" @click="abortModifying" class="btn btn-secondary">
                                        Annuler
                                </button>
                                <button @click="submitNewModifiedMovie"type="submit" class="btn btn-success">
                                    Confirmer les modifications et dupliquer
                                </button>
                            </div>
                        </form>
                    </div>
                </div>
            </div>
        </div>
</template>

<style scoped>
</style>