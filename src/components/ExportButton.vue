<script setup lang="ts">
import { type Movie } from "../scripts/movie";

const props = defineProps<{
    movieListToImport: Movie[]
}>();

const exportMovies = () => {
  exportMoviesToCSV(props.movieListToImport, "movies");
};

function exportMoviesToCSV(movies: (Movie)[], filename: string): void {
    if (movies.length === 0) return

    // Obtenir tous les en-têtes (Movie + DetailedMovie) en fusionnant les clés uniques
    const headers = Array.from(
        new Set(movies.flatMap(movie => Object.keys(movie)))
    )

    // Construire les lignes du CSV
    const rows = movies.map(movie =>
        headers.map(header => (movie as any)[header] ?? "").join(", ")
    )

    // Contenu du fichier CSV
    const csvContent = [headers.join(", "), ...rows].join("\n")

    // Création du fichier et téléchargement
    const blob = new Blob([csvContent], { type: "text/csv;charset=utf-8;" })
    const link = document.createElement("a")
    link.href = URL.createObjectURL(blob)
    link.setAttribute("download", `${filename}.csv`)
    document.body.appendChild(link)
    link.click()
    document.body.removeChild(link)
}
</script>

<template>
    <button 
        @click="exportMovies"
        class="btn text-white border border-info-subtle bg-info btn-outline-info px-4">
        <i class="bi bi-filetype-csv me-2"></i>Exporter en CSV
    </button>
</template>

<style scoped>
</style>