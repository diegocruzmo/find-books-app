<template>
  <form class="space-x-2">
    <input
      class="w-[60vw] rounded-md focus:outline-none focus-within:ring-zinc-900 focus:border-zinc-900"
      placeholder="Type book name, author..."
      type="search"
      v-model="query"
    />
    <button
      class="py-2 px-4 rounded-md bg-zinc-900 text-zinc-100 hover:bg-zinc-900/85"
      @click.prevent="$emit('search', { query })"
    >
      Search
    </button>
  </form>
</template>

<script setup>
import { ref } from 'vue'

const query = ref('')
</script>
