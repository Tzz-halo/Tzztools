<script setup>
import { defineProps } from 'vue'
import { ref, onMounted } from 'vue'
const DEFAULT_IMAGE_URL = '/default.png'
function handleImageError(event) {
  event.target.src = DEFAULT_IMAGE_URL
}

defineProps({
  cards: {
    type: Array,
    required: true,
    default: () => []
  }
})


</script>

<template>
  <div class="mt-6 md:mt-8 grid grid-cols-1 md:grid-cols-2 xl:grid-cols-3 gap-4 lg:gap-6">
    <div v-for="card in cards" :key="card.id" class="bg-white rounded-2xl p-6 hover:-translate-y-2 duration-200">
      <div class="flex justify-between items-start">
        <img class="w-16 h-16 rounded-xl"  @error="handleImageError" :src="card.imageUrl">
      </div>
      <div class="pt-6">
        <div class="flex items-center">
          <a class="text-slate-600 text-base font-semibold hover:underline" :href="card.link" target="_blank">{{
            card.title }}</a>
          <svg class="ml-1 stroke-slate-500" xmlns="http://www.w3.org/2000/svg" width="18" height="18"
            viewBox="0 0 24 24" fill="none" stroke="#000000" stroke-width="2" stroke-linecap="round"
            stroke-linejoin="round">
            <g fill="none" fill-rule="evenodd">
              <path d="M18 14v5a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2V8c0-1.1.9-2 2-2h5M15 3h6v6M10 14L20.2 3.8"></path>
            </g>
          </svg>
        </div>
        <p class="mt-2 text-xs text-slate-400 leading-normal h-8">{{ card.description }}</p>
        <div class="mt-4 flex items-center justify-between">
          <div class="bg-[#FFEBD1] text-[#FDC071] inline rounded-full text-xs px-4 py-1.5">{{ card.tag }}</div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped></style>
