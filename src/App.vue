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

onMounted(() => {
    // loadFromLocalStorage();
    cards.value = [
        {
            id: "1",
            content: "Es más fácil que React??",
            columnId: "todo",
        },
        {
            id: "2",
            content: "Terminar el prototipo",
            columnId: "done",
        },
        {
            id: "3",
            content: "Ya está quedando mejor",
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

const deleteCard = (cardId: string) => {
    cards.value = cards.value.filter((c) => c.id !== cardId);
};
</script>

<template>
    <section class="section pt-6 mr-2 ml-2">
        <div class="container">
            <h3 class="title is-3 has-text-centered has-text-primary">
                Hello KanbApp
            </h3>
            <h5 class="subtitle is-5 has-text-centered has-text-grey mb-6">
                A minimalist kanban board!
            </h5>

            <div class="columns is-centered">
                <KanbanColumn
                    v-for="col in columns"
                    v-bind:key="col.id"
                    v-bind:column="col"
                    v-bind:cards="getCardsByColumn(col.id)"
                    @drop="onDrop"
                    @dragover="onDragOver"
                    @add-card="(content) => addNewCard(content, col.id)"
                    @delete-card="deleteCard"
                />
                <!-- </div> -->
            </div>
        </div>
    </section>
</template>

<style scoped></style>
