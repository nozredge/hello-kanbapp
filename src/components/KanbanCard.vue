<script setup lang="ts">
defineProps<{
    title: string,
    description: string
}>()


const onDragStart = (e: DragEvent) => {
    const target = e.target as HTMLElement
    target.classList.add("is-dragging")
    e.dataTransfer?.setData("cardId", target.dataset.cardId!)
}

const onDragEnd = (e: DragEvent) => {
    const target = e.target as HTMLElement
    target.classList.remove("is-dragging")
}
</script>

<template>
    <div class="card mb-3" draggable="true" :data-card-id="title" @dragstart="onDragStart" @dragend="onDragEnd">
        <div class="card-content">
            <p class="title is-5 has-text-dark">{{ title }}</p>
            <p class="subtitle is-6 has-text-grey" v-if="description">{{ description }}</p>
        </div>
    </div>
</template>