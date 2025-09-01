<script setup>
import LibraryDB from '../data/books.json'
import Book from './Book.vue'
import { ref, computed } from 'vue'

const books = LibraryDB.library.map(items => items.book); //obtener libros del JSON
const totalBooks = books.length;

const genres = LibraryDB.library.map(items => items.book.genre); //obtener géneros de los libros
// Crear un array único de géneros. El método Set elimina duplicados.
const uniqueGenres = [...new Set(genres)];

const pages = LibraryDB.library.map(items => items.book.pages); //obtener páginas de los libros
// Crear un array único de páginas. El método Set elimina duplicados.
const uniquePages = [...new Set(pages)];
const maxpages = Math.max(...uniquePages); //obtener el valor máximo de páginas

//LOGICA

//- 1. Filtro de género y páginas

const filterSelect = ref('all'); // Valor por defecto del filtro de género
const currentPages = ref(maxpages);

const filteredBooks = computed(() => {
    if (filterSelect.value === 'all') {
        return books.filter(book => {
            return book.pages <= currentPages.value // Si el género es 'all', devuelve todos los libros con páginas menores o iguales al valor del slider
        });
    } else {
        return books.filter((book) => {
            return book.genre === filterSelect.value && book.pages <= currentPages.value
        });
    }
})



// - 2. Añadir a la lectura
const readingBook = ref([]);
const readingBooks = computed(() => {
    return [...new Set(readingBook.value)];
});

const addToReading = (book) => {
    readingBook.value.push(book); // Añade el libro al array de lectura
    console.log(`Añadido a la lectura: ${book.title}`);
    console.log('Libros en lectura:', readingBook.value);
}
// eliminar libro de la lectura
const removeFromReading = (book) => {
    readingBook.value = readingBook.value.filter(b => b.title !== book.title);
    console.log(`eliminado: ${book.title}`);
}


// - 3. Cantidad de libros en pantalla
const screenBooks = ref(0);
const screenBooksCount = computed(() => {
    screenBooks.value = filteredBooks.value.length;
    return screenBooks.value
})
//Modal
const showModal = ref(false)
const toggleModal = () => {
    showModal.value = !showModal.value
}


</script>
<template>
    <main>
        <section class="filter-container">
            <h2>Libros disponibles - {{ screenBooksCount }}</h2>
            <article class="filter-form">
                <div>
                    <label for="filter-select">Filtrar por género:</label>
                    <select v-model="filterSelect" id="filter-select">
                        <option value="all">Todos</option>
                        <option v-for="genre in uniqueGenres" :key="genre">{{ genre }}</option>
                    </select>
                </div>
                <div>
                    <label for="filter-pages">Filtrar por páginas - {{ currentPages }}</label>
                    <input type="range" id="filter-pages" min="0" :max="maxpages" step="50" v-model="currentPages">
                </div>
                <div>
                    <button @click="showModal = true" class="show-books-btn">Tus libros</button>
                </div>
            </article>
        </section>

        <hr>

        <section class="books-container">
            <h2>Libros</h2>
            <article class="books-list">
                <Book v-for="book in filteredBooks" :key="book.title" :book="book" :add-to-reading="addToReading" />
            </article>
        </section>


        <div v-if="showModal" class="modal-overlay" @click.self="toggleModal">
            <div class="modal">
                <header class="modal-header">
                    <h2>Libros en lectura</h2>
                    <button class="close-btn" @click="toggleModal">✖</button>
                </header>
                <hr>
                <br>
                <article class="books-list">
                    <div v-if="readingBook.length === 0" class="empty-reading">
                        <p>No tienes libros añadidos aún</p>
                    </div>

                    <Book v-else v-for="book in readingBooks" :key="book.title" :book="book"
                        :add-to-reading="addToReading" :isReading="true" :removeFromReading="removeFromReading" />
                </article>
            </div>
        </div>
    </main>
</template>


<style scoped>
main {
    padding: 1em;
}

hr {
    border: 0;
    border-top: 1px solid #ffffff0e;
}

.filter-container {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 20px;
    padding: 1.5em;
}

.books-container {
    padding: 1.5em;
}

.books-list {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
    gap: 8px;
}

.empty-reading {
    color: #888;
    font-style: italic;
    width: 100%;
}

.filter-form {
    display: flex;
    flex-wrap: wrap;
    gap: 1.5rem;
    align-items: center;
    margin-top: 1rem;
}

.filter-form label {
    display: block;
    font-weight: 600;
    margin-bottom: 0.3rem;
    font-size: 0.9rem;
    color: #ddd;
}

/* select */
.filter-form select {
    padding: 0.6em 1em;
    border-radius: 8px;
    border: 1px solid #555;
    background: #222;
    color: #fff;
    font-size: 0.95rem;
    cursor: pointer;
    transition: border 0.3s, box-shadow 0.3s;
}

.filter-form select:focus {
    outline: none;
    border: 1px solid #bb0000;
    box-shadow: 0 0 5px #bb0000;
}

/* Range (slider) */
.filter-form input[type="range"] {
    width: 220px;
    cursor: pointer;
    accent-color: #bb0000;
}

.show-books-btn {
    background-color: #222;
    color: #fff;
    font-weight: 600;
    border: none;
    padding: 0.7em 1.4em;
    border-radius: 8px;
    cursor: pointer;
    transition: transform 0.2s, box-shadow 0.3s;
}

.show-books-btn:hover {
    transform: translateY(-2px);
    box-shadow: 0 6px 12px rgba(0, 170, 255, 0.3);
}

.show-books-btn:active {
    transform: scale(0.96);
}
</style>