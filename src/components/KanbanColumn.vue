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
    <div class="columns">
        <div class="column">
            <div
                class="is-flex is-flex-direction-column is-justify-content-space-around box has-background-white-ter"
                style="
                    /* min-height: 30vh;
                    min-width: 20vw; */
                    border-radius: 16px;
                    border: 3px solid transparent;
                "
                :style="{
                    borderColor:
                        props.column.id === 'todo'
                            ? '#b8a6ff'
                            : props.column.id === 'doing'
                            ? '#a0d8ef'
                            : '#b5e2b2',
                }"
                @drop="emit('drop', $event, props.column.id)"
                @dragover="emit('dragover', $event)"
            >
                <h3
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
                </h3>
                <!-- Cards list -->
                <div class="mb-5">
                    <KanbanCard
                        v-for="card in props.cards"
                        :key="card.id"
                        :card="card"
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
                        <button
                            class="button is-primary is-small"
                            @click="addTask"
                        >
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
    </div>
</template>
