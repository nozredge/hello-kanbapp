<script setup lang="ts">
import KanbanCard from "./KanbanCard.vue";

const props = defineProps<{
    title: string,
    cards: { title: string; description: string }[]
}>()

const onDrop = (e: DragEvent) => {
    e.preventDefault()

    const cardTitle = e.dataTransfer?.getData("cardId")
    if (cardTitle) {
        console.log('Soltaste la tarjeta:', cardTitle, 'en la columna:', props.title)
    }

}

const onDragOver = (e: DragEvent) => {
    e.preventDefault()
}
</script>


<template>
    <div class="column is-one-third-widescreen is-full-mobile">
        <div class="box-has-background-white-ter" style="min-height: 500px; border-radius: 12px;" @drop="onDrop"
            @dragover="onDragOver">

            <h3 class="title is-4 has-text-centered mb-4 has-text-primary">
                {{ title }}
            </h3>

            <div class="mb-4">
                <KanbanCard v-for="card in cards" :key="card.title" :title="card.title"
                    :description="card.description" />
            </div>

            <button class="button is-small is-primary is-outlined is-fullwidth">
                +Add card
            </button>
        </div>
    </div>
</template>