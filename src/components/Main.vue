<script setup>
import LibraryDB from '../data/books.json'

const books = LibraryDB.library.map(items => items.book); //obtener libros del JSON
console.log(books); //verificar que se obtienen los libros
const genres = LibraryDB.library.map(items => items.book.genre); //obtener géneros de los libros
// Crear un array único de géneros. El método Set elimina duplicados.
const uniqueGenres = [...new Set(genres)];

const pages = LibraryDB.library.map(items => items.book.pages); //obtener páginas de los libros
// Crear un array único de páginas. El método Set elimina duplicados.
const uniquePages = [...new Set(pages)];
const maxpages = Math.max(...uniquePages); //obtener el valor máximo de páginas

</script>
<template>
    <main>
        <section class="filter-container">
            <h2>Libros disponibles - </h2>
            <article class="filter-form">
                <div>
                    <label for="filter-select">Filtrar por género:</label>
                    <select name="filter-select" id="filter-select">
                        <option value="all">Todos</option>
                        <option v-for="genre in uniqueGenres" :value="genre" :key="genre">{{ genre }}</option>
                    </select>
                </div>
                <div>
                    <label for="filter-pages">Filtrar por páginas</label>
                    <input type="range" name="filter-pages" id="filter-pages" min="0" :max="maxpages" step="50">
                </div>
            </article>
        </section>
        <hr>
        <section class="books-container">
            <h2>Libros</h2>
            <article class="books-list">
                <div class="book" v-for="book in books" :key="book.title">
                    <img :src="book.cover" alt="imagen del libro {{ book.title }}">
                    <h3>{{ book.title }}</h3>
                </div>
            </article>
        </section>
    </main>
</template>