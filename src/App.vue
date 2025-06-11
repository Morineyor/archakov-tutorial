<script setup>
import { computed, provide, ref, watch } from 'vue'
import axios from 'axios'

import headerComponent from './components/headerComponent.vue'
import DrawerPopUp from './components/DrawerPopUp.vue'

const cart = ref([])
const isCreatingOrder = ref(false)

const cartOpen = ref(false)

const totalPrice = computed(() =>
  cart.value.reduce((acc, item) => acc + item.price, 0),
)

const vatPrice = computed(() => Math.round((totalPrice.value * 5) / 100))

const cartIsEmpty = computed(() => cart.value.length === 0)

const cartButtonDisabled = computed(
  () => isCreatingOrder.value || cartIsEmpty.value,
)

const closeCart = () => {
  cartOpen.value = false
}

const openCart = () => {
  cartOpen.value = true
}

const addToCart = (item) => {
  cart.value.push(item)
  item.isAdded = true
}

const removeFromCart = (item) => {
  cart.value.splice(cart.value.indexOf(item), 1)
  item.isAdded = false
}

const createOrder = async () => {
  try {
    isCreatingOrder.value = true
    const { data } = axios.post('https://614c7a5111e51c55.mokky.dev/orders', {
      items: cart.value,
      totalPrice: totalPrice.value,
      vatPrice: vatPrice.value,
    })

    cart.value = []

    return data
  } catch (err) {
    console.log(err)
  } finally {
    isCreatingOrder.value = false
  }
}

watch(
  cart,
  () => {
    localStorage.setItem('cart', JSON.stringify(cart.value))
  },
  {
    deep: true,
  },
)

provide('cart', {
  cart,
  closeCart,
  openCart,
  addToCart,
  removeFromCart,
})
</script>

<template>
  <DrawerPopUp
    v-if="cartOpen"
    :total-price="totalPrice"
    :vat-price="vatPrice"
    @close-cart="closeCart"
    @create-order="createOrder"
    :cart-button-disabled="cartButtonDisabled"
  />
  <div class="bg-white w-4/5 m-auto rounded-xl shadow-xl mt-14">
    <headerComponent
      @open-cart="openCart"
      :total-price="totalPrice"
    />

    <div class="p-10">
      <router-view> </router-view>
    </div>
  </div>
</template>

<style scoped></style>
