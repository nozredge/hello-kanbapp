<script setup lang="ts">
import { ref } from "vue";
import KanbanCard from "./KanbanCard.vue";

const isAdding = ref(false);
const newTask = ref("");

const props = defineProps<{
    column: { id: string; title: string };
    cards: { id: string; content: string }[];
}>();

const emit = defineEmits<{
    (e: "drop", event: DragEvent, columnId: string): void;
    (e: "dragover", event: DragEvent): void;
    (e: "add-card", content: string): void;
    (e: "delete-card", cardId: string): void;
}>();

const addTask = () => {
    const text = newTask.value.trim();
    if (text) {
        // El camarero grita "add-card" para llamar la atención
        // y le pasa "text"
        emit("add-card", text);
        newTask.value = "";
    }
    isAdding.value = true;
};
</script>

<template>
    <div
        class="column is-one-third-tablet is-one-quarter-desktop is-full-mobile"
    >
        <div
            class="box has-background-white-ter"
            style="border-radius: 16px; border: 2px solid transparent"
            :style="{
                borderColor: 'rgba(165, 231, 206, 1)',
            }"
            @drop="emit('drop', $event, props.column.id)"
            @dragover="emit('dragover', $event)"
        >
            <h4
                class="title is-4 has-text-centered mb-5"
                :class="{
                    'has-text-primary': props.column.id === 'todo',
                    'has-text-info': props.column.id === 'doing',
                    'has-text-success': props.column.id === 'done',
                }"
            >
                {{ props.column.title }}
                <span class="tag is-rounded ml-2">
                    {{ props.cards.length }}</span
                >
            </h4>
            <!-- Cards list -->
            <div class="mb-5">
                <KanbanCard
                    v-for="card in props.cards"
                    v-bind:key="card.id"
                    v-bind:card="card"
                    v-bind:column-id="props.column.id"
                    @delete="emit('delete-card', $event)"
                />
            </div>

            <!-- input inline o boton -->
            <div v-if="isAdding">
                <div class="field">
                    <div class="control">
                        <textarea
                            class="textarea is-small has-fixed-size"
                            rows="2"
                            placeholder="Escribe tu tarea..."
                            v-model="newTask"
                            @keyup.enter.ctrl="addTask"
                            autofocus
                        ></textarea>
                    </div>
                </div>
                <div class="buttons is-right">
                    <button
                        class="button is-small"
                        @click="
                            isAdding = false;
                            newTask = '';
                        "
                    >
                        Cancelar
                    </button>
                    <button class="button is-primary is-small" @click="addTask">
                        Añadir
                    </button>
                </div>
            </div>

            <button
                v-else
                class="button is-fullwidth is-outlined is-primary has-text-weight-bold mt-3"
                @click="isAdding = true"
            >
                + Añadir tarea
            </button>
        </div>
    </div>
</template>

<style scoped>
.column {
    /* max-width: 400px; */
}
</style>
