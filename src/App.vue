<script setup lang="ts">
import { onMounted, ref } from 'vue';
import KanbanCard from './components/KanbanCard.vue';
import KanbanColumn from './components/KanbanColumn.vue';

interface Card {
    id: string
    title: string
    description: string
    columnId: string
}

interface Column {
    id: string
    title: string
}

const cards = ref<Card[]>([])

const columns = ref<Column[]>([
    { id: 'todo', title: 'To Do' },
    { id: 'doing', title: 'Doing' },
    { id: 'done', title: 'Done' }
])

const getCardsByColumns = (columnId: string) => cards.value.filter(card => card.columnId === columnId)

// Drag and drop
const onDrop = (e: DragEvent, targetColumnId: string) => {
    e.preventDefault()

    const cardId = e.dataTransfer?.getData('cardId')
    if (!cardId) return


    // Move card to a new column
    const card = cards.value.find(c => c.id === cardId)
    if (card) {
        card.columnId = targetColumnId
        saveToLocalStorage()
    }
}

const onDragOver = (e: DragEvent) => {
    e.preventDefault()
}

const saveToLocalStorage = () => {
    localStorage.setItem("kanbapp-cards", JSON.stringify(cards.value))
}

const loadFromLocalStorage = () => {
    const saved = localStorage.getItem("kanbapp-cards")
    if (saved) {
        cards.value = JSON.parse(saved)
    }

    else {
        cards.value = [
            { id: '1', title: 'Aprender Vue 3', description: 'Es más fácil que React', columnId: 'todo' },
            { id: '2', title: 'Hacer este proyecto', description: 'Y dejar al profe flipando', columnId: 'todo' },
            { id: '3', title: 'Diseñar colores pastel', description: 'Ya está quedando precioso', columnId: 'doing' }
        ]
    }
}

onMounted(() => {
    loadFromLocalStorage()
})


// type KanbanColumnsType = {
//     id: string;
//     title: string;
//     cards: {
//         title: string;
//         description: string;
//     }[];
// }

// const columns = ref<KanbanColumnsType[]>([
//     {
//         id: "todo",
//         title: "To Do",
//         cards: [
//             {
//                 title: "Aprender Vue 3 al vuelo",
//                 description: "Es más fácil"
//             },
//         ]
//     },

//     {
//         id: "doing",
//         title: "Doing",
//         cards: [
//             {
//                 title: "Programar el KanbApp",
//                 description: "...con la ayuda de Grok y Gemini"
//             }
//         ]
//     },
//     {
//         id: "done",
//         title: "Done",
//         cards: []
//     }
// ])
</script>

<template>
    <section class="section">
        <div class="container">
            <h1 class="title has-text-centered has-text-primary" style="font-size: 3rem; font-weight: 800">
                Hello KanbApp
            </h1>
            <p class="subtitle has-text-centered has-text-grey mb-6">
                A minimalist kanban board!
            </p>

            <!-- Kanban here -->
            <div class="columns is-mobile is-multiline is-centered">
                <!-- Columns here -->
                <KanbanColumn v-for="col in columns" :key="col.id" :title="col.title" :cards="col.cards" />
            </div>
        </div>
    </section>
</template>

<style scoped></style>
