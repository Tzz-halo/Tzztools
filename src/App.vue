<script setup>
import { ref, onMounted, computed } from 'vue'
import Hander from './components/Hander.vue'
import Tools from './components/Tools.vue'
import Footer from './components/Footer.vue'

const allTools = ref([])
const filteredTools = ref([])
const selectedTag = ref('全部')

async function loadTools() {
  try {
    const response = await fetch('/data.json')
    if (!response.ok) throw new Error('加载工具数据失败')
    const data = await response.json()
    allTools.value = data
    filteredTools.value = data
  } catch (error) {
    console.error('加载工具数据出错:', error)
  }
}

function performSearch(query) {
  if (!query.trim()) {
    filteredTools.value = allTools.value
    return
  }

  const lowerQuery = query.toLowerCase()
  filteredTools.value = allTools.value.filter(card =>
    card.title.toLowerCase().includes(lowerQuery) ||
    card.description.toLowerCase().includes(lowerQuery) ||
    card.tag.toLowerCase().includes(lowerQuery)
  )
}

function filterByTag(tag) {
  selectedTag.value = tag
  if (tag === '全部') {
    filteredTools.value = allTools.value
    return
  }

  filteredTools.value = allTools.value.filter(card => card.tag === tag)
}

onMounted(() => {
  loadTools()
})


// 动态生成标签
const uniqueTags = computed(() => {
  // 提取所有非空 tag 并去重
  const tags = allTools.value
    .map(tool => tool.tag)
    .filter(tag => tag); // 过滤空值

  // 使用 Set 去重并转换为数组
  return ['全部', ...Array.from(new Set(tags))];
});


</script>

<template>
  <Hander @search="performSearch" />
  <div class="container mx-auto px-4 flex flex-col mb-8">
    <!-- App.vue 动态生成标签 -->
    <div>
      <div class="mt-4 sm:mt-6 flex flex-wrap justify-center space-x-5 text-sm">
        <a v-for="tag in uniqueTags" :key="tag" @click="filterByTag(tag)" :class="{
          'bg-slate-700 text-white': selectedTag === tag,
          'bg-slate-50 text-gray-400': selectedTag !== tag
        }"
          class="hover:bg-slate-200 w-auto mt-2 px-5 py-1 border border-transparent rounded-full text-base font-normal transition ease-in-out duration-300 cursor-pointer">
          {{ tag }}{{ tag === '全部' ? `(${allTools.length})` : '' }}
        </a>
      </div>

      <Tools :cards="filteredTools" />
      <Footer></Footer>
    </div>
  </div>
</template>

<style scoped></style>
