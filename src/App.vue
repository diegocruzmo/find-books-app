<template>
  <NavBar />
  <section class="flex flex-col items-center justify-center my-12">
    <h2 class="text-xl md:text-2xl font-semibold mb-6">
      Find the book you want.
    </h2>
    <SearchForm @search="search" />
  </section>
  <div class="w-full text-center" v-if="isLoading">
    <LoadSpinner />
  </div>
  <section class="w-[90vw] md:w[8-vw] mx-auto mb-8" v-else>
    <h1 class="text-xl font-semibold" v-if="books">
      {{ `${books.totalItems} books found` }}
    </h1>
    <div class="flex flex-col items-center" v-else>
      <img
        src="./assets/detective.svg"
        class="w-36 h-36 mb-4"
        alt="search images"
      />
      <p>Waiting for search</p>
    </div>
    <div class="grid grid-cols1 lg:grid-cols-2 gap-4" v-if="books">
      <BookCard v-for="{ volumeInfo, id } in books.items" :key="id">
        <template #header>
          <figure>
            <img
              :src="volumeInfo?.imageLinks?.thumbnail"
              class="w-full h-full md:w-48 object-cover"
              alt="book"
            />
          </figure>
        </template>
        <template #content>
          <h3 class="font-bold md:text-lg">{{ volumeInfo?.title }}</h3>
          <p class="text-small">
            Author: {{ volumeInfo?.authors?.toString() }}
          </p>
          <p class="text-small">Publisher: {{ volumeInfo?.publisher }}</p>
          <p class="text-small">
            Published Date: {{ volumeInfo?.publishedDate }}
          </p>
          <a
            :href="volumeInfo?.infoLink"
            taget="_blank"
            rel="noopener"
            class="inline-block py-2 px-4 bg-zinc-100 font-semibold rounded-md hover:bg-zinc-100/90"
            >More Info</a
          >
        </template>
      </BookCard>
    </div>
  </section>
</template>

<script setup>
import NavBar from './components/NavBar.vue'
import SearchForm from './components/SearchForm.vue'
import BookCard from './components/BookCard.vue'
import LoadSpinner from './components/LoadSpinner.vue'
import { ref } from 'vue'

const books = ref(null)
const isLoading = ref(false)

async function search({ query }) {
  if (!query) return
  const url = new URL(import.meta.env.VITE_BASE_URL)
  url.searchParams.set('q', query)

  isLoading.value = true
  const res = await fetch(url)
  const data = await res.json()
  isLoading.value = false
  books.value = { items: data.items, totalItems: data.totalItems }
}
</script>
