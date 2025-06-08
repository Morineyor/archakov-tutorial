<script setup>
import CardComponent from './CardComponent.vue'

const onClickAdd = () => {
  alert('Добавить!')
}
</script>

<template>
  <div class="grid grid-cols-4 gap-5">
    <CardComponent
      title="Мужские Кроссовки Nike Blazer Mid Suede"
      image-url="/sneakers/sneakers-1.jpg"
      :price="12999"
      :is-favorite="true"
      :is-added="false"
      :onClickAdd="onClickAdd"
    />
  </div>
</template>
