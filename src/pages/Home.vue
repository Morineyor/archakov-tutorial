<script setup>
import { inject, onMounted, reactive, ref, watch } from 'vue'
import debounce from 'lodash.debounce'
import CardList from '../components/CardList.vue'
import axios from 'axios'

const { cart, addToCart, removeFromCart } = inject('cart')

const items = ref([])

const filters = reactive({
  sortBy: 'title',
  searchQuery: '',
})

const onClickAddPlus = (item) => {
  if (!item.isAdded) {
    addToCart(item)
  } else {
    removeFromCart(item)
  }
}

const onChangeSelect = (event) => {
  filters.sortBy = event.target.value
}

const onChangeSearchInput = debounce((event) => {
  filters.searchQuery = event.target.value
}, 250)

let isProcessing = false

const addToFavorite = async (item) => {
  if (isProcessing) return

  isProcessing = true
  try {
    if (!item.isFavorite) {
      const obj = { item_id: item.id }
      item.isFavorite = true
      const { data } = await axios.post(
        'https://614c7a5111e51c55.mokky.dev/favorites',
        obj,
      )
      item.favoriteId = data.id
    } else {
      item.isFavorite = false
      await axios.delete(
        `https://614c7a5111e51c55.mokky.dev/favorites/${item.favoriteId}`,
      )
      item.favoriteId = null
    }
  } catch (err) {
    console.log(err)
    item.isFavorite = !item.isFavorite
  } finally {
    isProcessing = false
  }
}

const fetchItems = async () => {
  try {
    const params = {
      sortBy: filters.sortBy,
    }

    if (filters.searchQuery) {
      params.title = `*${filters.searchQuery}*`
    }

    const { data } = await axios.get(
      `https://604781a0efa572c1.mokky.dev/items`,
      { params },
    )

    items.value = data.map((obj) => ({
      ...obj,
      isFavorite: false,
      favoriteId: null,
      isAdded: false,
    }))
  } catch (err) {
    console.log(err)
  }
}

const fetchFavorites = async () => {
  try {
    const { data: favorites } = await axios.get(
      'https://614c7a5111e51c55.mokky.dev/favorites',
    )

    items.value = items.value.map((item) => {
      const favorite = favorites.find(
        (favorite) => favorite.item_id === item.id,
      )

      if (!favorite) {
        return item
      }

      return {
        ...item,
        isFavorite: true,
        favoriteId: favorite.id,
      }
    })
  } catch (err) {
    console.log(err)
  }
}

onMounted(async () => {
  const localCart = localStorage.getItem('cart')
  cart.value = localCart ? JSON.parse(localCart) : []

  await fetchItems()
  await fetchFavorites()

  items.value = items.value.map((item) => ({
    ...item,
    isAdded: cart.value.some((cartItem) => cartItem.id === item.id),
  }))
})

watch(cart, () => {
  items.value = items.value.map((item) => ({
    ...item,
    isAdded: false,
  }))
})

watch(filters, fetchItems)
</script>

<template>
  <div class="flex justify-between items-center">
    <h2 class="text-3xl font-bold mb-8">Все Кроссовки</h2>

    <div class="flex gap-4">
      <select
        @change="onChangeSelect"
        class="py-2 px-3 border border-gray-400 rounded-md outline-none"
        name=""
        id=""
      >
        <option value="name">По названию</option>
        <option value="price">По цене (Дешевые)</option>
        <option value="-price">По цене (Дорогие)</option>
      </select>

      <div class="relative">
        <img
          class="absolute left-4 top-3.5"
          src="/search.svg"
          alt="Search"
        />
        <input
          @input="onChangeSearchInput"
          class="border border-gray-200 rounded-md py-2 pl-12 pr-4 outline-none focus:border-gray-400 transition"
          placeholder="Поиск..."
          type="text"
          name=""
          id=""
        />
      </div>
    </div>
  </div>
  <div class="mt-10">
    <CardList
      :items="items"
      @add-to-favorite="addToFavorite"
      @add-to-cart="onClickAddPlus"
    />
  </div>
</template>
