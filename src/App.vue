<script setup lang="ts">
import { onMounted, ref } from "vue";
import KanbanColumn from "./components/KanbanColumn.vue";

interface Card {
    id: string;
    content: string;
    columnId: string;
}

interface Column {
    id: string;
    title: string;
}

const cards = ref<Card[]>([]);

const columns = ref<Column[]>([
    { id: "todo", title: "To Do" },
    { id: "doing", title: "Doing" },
    { id: "done", title: "Done" },
]);

const getCardsByColumn = (columnId: string) =>
    cards.value.filter((card) => card.columnId === columnId);

// Drag and drop
const onDrop = (e: DragEvent, targetColumnId: string) => {
    e.preventDefault();

    const cardId = e.dataTransfer?.getData("cardId");
    if (!cardId) return;

    // Move card to a new column
    const card = cards.value.find((c) => c.id === cardId);
    if (card) {
        card.columnId = targetColumnId;
        // saveToLocalStorage();
    }
};

const onDragOver = (e: DragEvent) => {
    e.preventDefault();
};

// const saveToLocalStorage = () => {
//     localStorage.setItem("kanbapp-cards", JSON.stringify(cards.value));
// };

// const loadFromLocalStorage = () => {
//     const saved = localStorage.getItem("kanbapp-cards");
//     if (saved) {
//         cards.value = JSON.parse(saved);
//     } else {
//         cards.value = [
//             {
//                 id: "1",
//                 content: "Es más fácil que React",
//                 columnId: "todo",
//             },
//             {
//                 id: "2",
//                 content: "Y dejar al profe flipando",
//                 columnId: "todo",
//             },
//             {
//                 id: "3",
//                 content: "Ya está quedando precioso",
//                 columnId: "doing",
//             },
//         ];
//     }
// };

onMounted(() => {
    // loadFromLocalStorage();
    cards.value = [
        {
            id: "1",
            content: "Es más fácil que React",
            columnId: "todo",
        },
        {
            id: "2",
            content: "Y dejar al profe flipando",
            columnId: "todo",
        },
        {
            id: "3",
            content: "Ya está quedando precioso",
            columnId: "doing",
        },
    ];
});

const addNewCard = (content: string, columnId: string) => {
    const newCard: Card = {
        id: Date.now().toString(),
        content,
        columnId,
    };

    cards.value.push(newCard);
};
</script>

<template>
    <section class="section pt-6">
        <div class="container">
            <h1
                class="title has-text-centered has-text-primary"
                style="font-size: 3rem; font-weight: 800"
            >
                Hello KanbApp
            </h1>
            <p class="subtitle has-text-centered has-text-grey is-size-5 mb-6">
                A minimalist kanban board!
            </p>

            <!-- Kanban here -->
            <div
                class="columns is-mobile is-multiline is-centered"
                style="gap: 1.5rem"
            >
                <!-- Columns here -->
                <KanbanColumn
                    v-for="col in columns"
                    :key="col.id"
                    :column="col"
                    :cards="getCardsByColumn(col.id)"
                    @drop="onDrop"
                    @dragover="onDragOver"
                    @add-card="(content) => addNewCard(content, col.id)"
                />
            </div>
        </div>
    </section>
</template>

<style scoped></style>
