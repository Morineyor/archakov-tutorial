<script setup>
import CardComponent from './CardComponent.vue'

defineProps({
  items: Array,
})

const emit = defineEmits(['addToFavorite', 'addToCart'])
</script>

<template>
  <div
    v-auto-animate
    class="grid grid-cols-4 gap-5"
  >
    <CardComponent
      v-for="item in items"
      :key="item.id"
      :id="item.id"
      :title="item.title"
      :image-url="item.imageUrl"
      :price="item.price"
      :is-favorite="item.isFavorite"
      :is-added="item.isAdded"
      :onClickAdd="() => emit('addToCart', item)"
      :onClickFavorite="() => emit('addToFavorite', item)"
    />
  </div>
</template>
