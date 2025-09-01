<script setup>
const { book, addToReading, isReading, deleteBook } = defineProps({
    book:{
        type: Object,
        required: true
    },
    addToReading: {
        type: Function,
        required: false
    },
    isReading: {
        type: Boolean,
        required: false,
        default: false
    },
    removeFromReading: {
        type: Function,
        required: false
    }
})
</script>

<template>
    <div class="book" :key="book.title">
        <img :src="book.cover" :alt="`Imagen del libro ${book.title}`" />
        <div class="book-info">
            <h3>{{ book.title }}</h3>
            <p>Autor: {{ book.author.name }}</p>
            <p>{{ book.genre }}</p>
            <p>Páginas: {{ book.pages }}</p>
        </div>
        <div class="book-btn">
            <button v-if="isReading === false" class="btn-primary" @click="addToReading(book)">Leer</button>
            <button v-else class="btn-primary" @click="removeFromReading(book)">Eliminar</button>
            <button class="btn-secondary">Descargar</button>
        </div>
    </div>
</template>

<style scoped>
.book {
    padding: 1em;
    background-color: #222222;
    box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
    width: auto;
    border-radius: 5px;
    display: flex;
    flex-direction: column;
    justify-content: space-between;

    img {
        width: 100%;
        height: auto;
    }

    .book-info {
        margin: 1em 0;
        color: #f0f0f0;
    }

    .btn-primary,
    .btn-secondary {
        margin: 5px 2px;
        padding: 0.5em 1em;
        border: none;
        border-radius: 5px;
        cursor: pointer;
        transition: opacity 0.3s ease;
    }

    .btn-primary {
        background-color: #bb0000;
        color: white;
    }

    .btn-secondary {
        background-color: #3a3a3a;
        color: white;
    }

    .btn-primary:hover,
    .btn-secondary:hover {
        opacity: 0.8;
    }
}
</style>
