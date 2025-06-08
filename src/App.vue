<script setup>
import headerComponent from './components/headerComponent.vue'
import CardList from './components/CardList.vue'
import DrawerPopUp from './components/DrawerPopUp.vue'
</script>

<template>
  <DrawerPopUp />
  <div class="bg-white w-4/5 m-auto rounded-xl shadow-xl mt-14">
    <headerComponent />

    <div class="p-10">
      <h2 class="text-3xl font-bold mb-8">Все Кроссовки</h2>

      <CardList />
    </div>
  </div>
</template>

<style scoped></style>
