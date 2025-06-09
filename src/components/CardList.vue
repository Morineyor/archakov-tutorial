<script setup>
import CardComponent from './CardComponent.vue'

defineProps({
  items: Array,
})

const onClickAdd = () => {
  alert('Добавить!')
}
</script>

<template>
  <div class="grid grid-cols-4 gap-5">
    <CardComponent
      v-for="item in items"
      :key="item.id"
      :title="item.title"
      :image-url="item.imageUrl"
      :price="item.price"
      :is-favorite="item.isFavorite"
      :is-added="false"
      :onClickAdd="onClickAdd"
    />
  </div>
</template>
