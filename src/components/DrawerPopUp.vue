<script setup>
const emit = defineEmits('closeCart', 'createOrder')
defineProps({
  totalPrice: Number,
  vatPrice: Number,
  cartButtonDisabled: Boolean,
})

import DrawerHeader from './DrawerHeader.vue'
import CartListItem from './CartListItem.vue'
import infoBlock from './infoBlock.vue'
</script>

<template>
  <div
    @click="() => emit('closeCart')"
    class="fixed top-0 left-0 h-full w-full z-10 bg-black opacity-60"
  ></div>
  <div class="bg-white w-96 h-full fixed right-0 top-0 z-20 p-8">
    <DrawerHeader />

    <div
      v-if="!totalPrice"
      class="flex h-full items-center"
    >
      <infoBlock
        title="Корзина пуста"
        description="Добавьте хотя бы одну пару кроссовок, чтобы сделать заказ."
        image-url="/package-icon.png"
      />
    </div>

    <CartListItem />

    <div
      v-if="totalPrice"
      class="flex flex-col gap-4 mt-7"
    >
      <div class="flex gap-2">
        <span>Итого:</span>
        <div class="flex-1 border-b border-dashed"></div>
        <b>{{ totalPrice }} Руб.</b>
      </div>

      <div
        v-if="totalPrice"
        class="flex gap-2"
      >
        <span>Налог 5%:</span>
        <div class="flex-1 border-b border-dashed"></div>
        <b>{{ vatPrice }} Руб.</b>
      </div>
      <button
        :disabled="cartButtonDisabled"
        @click="() => emit('createOrder')"
        class="bg-lime-500 w-full rounded-xl py-3 text-white disabled:bg-slate-400 disabled:cursor-default cursor-pointer hover:bg-lime-600 transition active:bg-lime-700 mt-4"
      >
        Оформить заказ
      </button>
    </div>
  </div>
</template>
