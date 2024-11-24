<script setup lang="ts">
import Folders from "./Folders.vue";
import {IFolder} from "../../types";
import {ref, watch} from "vue";

defineProps<{ folders: IFolder[], name:string }>()
const value = defineModel()
const isOpen = ref<boolean>(false)

function toggleOpen(): void {
  isOpen.value = !isOpen.value
}

watch(value, () => {
  isOpen.value = !isOpen.value
})
</script>

<template>
  <div class="relative">
    <button
        class="text-white bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:outline-none focus:ring-blue-300 font-medium rounded-lg text-sm px-5 py-2.5 text-center inline-flex items-center"
        type="button"
        @click="toggleOpen"
    >Папки
      <svg class="w-2.5 h-2.5 ms-3" aria-hidden="true" xmlns="http://www.w3.org/2000/svg" fill="none"
           viewBox="0 0 10 6">
        <path stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="m1 1 4 4 4-4"/>
      </svg>
    </button>
    <div v-if="isOpen"
         class="absolute top-[calc(100%+10px)] z-10 bg-white divide-y divide-gray-100 rounded-lg shadow w-44">
      <Folders :folders="folders" v-model="value" :name="name"/>
    </div>
  </div>
</template>
