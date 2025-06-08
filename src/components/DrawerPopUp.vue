<script setup>
import DrawerHeader from './DrawerHeader.vue'
import CartListItem from './CartListItem.vue'
</script>

<template>
  <div class="fixed top-0 left-0 h-full w-full z-10 bg-black opacity-60"></div>
  <div class="bg-white w-96 h-full fixed right-0 top-0 z-20 p-8">
    <DrawerHeader />
    <CartListItem />

    <div class="flex flex-col gap-4 mt-7">
      <div class="flex gap-2">
        <span>Итого:</span>
        <div class="flex-1 border-b border-dashed"></div>
        <b>12900 Руб.</b>
      </div>

      <div class="flex gap-2">
        <span>Налог 5%:</span>
        <div class="flex-1 border-b border-dashed"></div>
        <b>900 Руб.</b>
      </div>
      <button
        disabled="true"
        class="bg-lime-500 w-full rounded-xl py-3 text-white disabled:bg-slate-400 disabled:cursor-default cursor-pointer hover:bg-lime-600 transition active:bg-lime-700 mt-4"
      >
        Оформить заказ
      </button>
    </div>
  </div>
</template>
