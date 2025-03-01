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
        description: props.selectedMovie.description,
        category: props.selectedMovie.category,
        stock: props.selectedMovie.stock
    }
);

const emit = defineEmits<{
    (event: 'submit', movie: Movie): void
}>();

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
        stock: 0
    };
};

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
</script>

<template>
    {{ selectedMovie }}
    <form>
        <h3>Modifier le film</h3>
        <div class="mb-3">
            <label for="movieName" class="form-label">Nom du film</label>
            <input type="text" class="form-control" id="movieName" placeholder="Entrez le nom du film à modifier"
            v-model="selectedMovie.name">
        </div>
        <div class="mb-3">
            <label for="movieDescription" class="form-label">Description</label>
            <input type="text" class="form-control" id="movieDescription" placeholder="Entrez le description du film à modifier"
            v-model="selectedMovie.description">
            </div>
            <div class="mb-3">
                <label for="movieCategory" class="form-label">Catégorie</label>
                <select class="form-select" id="movieCategory" v-model="selectedMovie.category">
                    <option value="" disabled>Sélectionnez une nouvelle catégorie</option>
                    <option v-for="category in MovieCategoryEnum" 
                        :key="category" 
                        :value="category">
                        {{ category }}
                    </option>
                </select>
            </div>
            <div class="mb-3">
                <label for="movieStock" class="form-label">Stock</label>
                <input type="text" class="form-control" id="movieStock" placeholder="Entrez le stock du film à modifier"
                v-model.number="selectedMovie.stock">
            </div>
            <button  type="button" class="btn btn-success"
                @click="submitNewModifiedMovie">
                Enregistrer les modifications
            </button>
            <button type="button" class="btn btn-secondary ms-2"
            @click="resetMovie">
            Annuler
        </button>
        </form>
</template>

<style scoped>
</style>