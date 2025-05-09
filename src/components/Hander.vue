<script setup>
import { ref } from 'vue'
const searchQuery = ref('')
const emit = defineEmits(['search'])

function handleSearch() {
  const query = searchQuery.value.trim()
  if (query) {
    emit('search', query)
  }
}
</script>

<template>
  <div class="flex flex-wrap items-center top-0 sticky z-10 py-5 bg-slate-mbl">
    <div>
      <div style="font-size: larger;" class="flex items-center text-slate-600 text-base font-semibold">
        <span>TZZ ✨ TOOLS </span>
      </div>

    </div>
    <div class="ml-auto">
      <div class="flex items-center">
        <input v-model="searchQuery" @keyup.enter="handleSearch"
          class="w-32 px-4 py-1 ml-5 border border-transparent rounded-full text-base font-medium text-gray-400 bg-white"
          type="search" placeholder="🔍 搜索" />
      </div>
    </div>
  </div>

</template>

<style scoped></style>
