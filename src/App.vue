<script setup>
import { onMounted, reactive, ref, watch } from 'vue'
import axios from 'axios'

import headerComponent from './components/headerComponent.vue'
import CardList from './components/CardList.vue'
import DrawerPopUp from './components/DrawerPopUp.vue'

const items = ref([])

const filters = reactive({
  sortBy: 'title',
  searchQuery: '',
})

const onChangeSelect = (event) => {
  filters.sortBy = event.target.value
}

const onChangeSearchInput = (event) => {
  filters.searchQuery = event.target.value
}

const fetchFavorites = async () => {
  try {
    const { data: favorites } = await axios.get(
      'https://614c7a5111e51c55.mokky.dev/favorites',
    )

    items.value = items.value.map((item) => {
      const favorite = favorites.find(
        (favorite) => favorite.parentId === item.id,
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

const addToFavorite = async () => {
  item.isFavorite = true
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
      isAdded: false,
    }))
  } catch (err) {
    console.log(err)
  }
}

onMounted(async () => {
  await fetchItems()
  await fetchFavorites()
})

watch(filters, fetchItems)
</script>

<template>
  <!-- <DrawerPopUp /> -->
  <div class="bg-white w-4/5 m-auto rounded-xl shadow-xl mt-14">
    <headerComponent />

    <div class="p-10">
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
        <CardList :items="items" />
      </div>
    </div>
  </div>
</template>

<style scoped></style>
