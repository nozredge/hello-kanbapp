<script setup lang="ts">
const props = defineProps<{
    card: { id: string; content: string };
}>();

const onDragStart = (e: DragEvent) => {
    const target = e.target as HTMLElement;
    target.classList.add("is-dragging");
    // Save real ID of card
    e.dataTransfer?.setData("cardId", props.card.id);
};

const onDragEnd = (e: DragEvent) => {
    const target = e.target as HTMLElement;
    target.classList.remove("is-dragging");
};
</script>

<template>
    <div
        class="card mb-4 cursor-move is-draggable"
        draggable="true"
        @dragstart="onDragStart"
        @dragend="onDragEnd"
    >
        <div class="card-content py-4">
            <p class="title is-5 has-text-grey-darker" v-if="card.content">
                {{ card.content }}
            </p>
        </div>
    </div>
</template>

<style scoped>
.is-dragging {
    opacity: 0.5;
    transform: rotate(3deg) scale(0.95);
    transition: all 0.2s;
}

.cursor-move {
    cursor: move;
}
</style>
