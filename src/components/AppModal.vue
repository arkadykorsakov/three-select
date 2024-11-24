<script setup lang="ts">
import {IFolder} from "../types";
import {useFocusTrap} from "../hooks/useFocusTrap.ts";
import {ref} from "vue";
import AppThreeSelect from '../components/AppThreeSelect'

interface IProps {
  title: string
  folders: IFolder[],
  activeFolder: string,
  name: string
}

const props = defineProps<IProps>()
defineEmits<{
  (e: 'close'): void,
  (e: 'select', folderId: string): void
}>()

const {trapRef} = useFocusTrap()
const folder = ref(props.activeFolder)
</script>

<template>
  <div tabindex="-1" aria-hidden="true"
       class="overflow-y-auto overflow-x-hidden fixed top-0 right-0 left-0 z-50 bg-black bg-opacity-50 flex justify-center items-center w-full md:inset-0 min-h-dvh"
       @click.self="$emit('close')" @keydown.esc="$emit('close')" ref="trapRef">
    <div class="relative p-4 w-full max-w-2xl max-h-full">
      <div class="relative bg-white rounded-lg shadow">
        <div class="flex items-center justify-between p-4 md:p-5 border-b rounded-t">
          <h3 class="text-xl font-semibold text-gray-900">
            {{ title }}
          </h3>
          <button type="button"
                  class="text-gray-400 bg-transparent hover:bg-gray-200 hover:text-gray-900 rounded-lg text-sm w-8 h-8 ms-auto inline-flex justify-center items-center"
                  @click="$emit('close')"
                  tabindex="1"
          >
            <svg class="w-3 h-3" aria-hidden="true" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 14 14">
              <path stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                    d="m1 1 6 6m0 0 6 6M7 7l6-6M7 7l-6 6"/>
            </svg>
            <span class="sr-only">Close modal</span>
          </button>
        </div>
        <div class="p-4 md:p-5 space-y-4">
          <div class="mb-4 w-auto bg-blue-500 text-white p-4" v-if="folder">FOLDER ID: {{ folder }}</div>
          <AppThreeSelect :name="name" v-model="folder" :folders="folders"/>
        </div>
        <div class="flex items-center p-4 md:p-5 border-t border-gray-200 rounded-b">
          <button type="button"
                  :disabled="!folder"
                  @click="$emit('select', folder)"
                  class="text-white bg-blue-700 hover:bg-blue-800 disabled:opacity-50 focus:ring-4 focus:outline-none focus:ring-blue-300 font-medium rounded-lg text-sm px-5 py-2.5 text-center">
            ОК
          </button>
          <button type="button"
                  class="py-2.5 px-5 ms-3 text-sm font-medium text-gray-900 focus:outline-none bg-white rounded-lg border border-gray-200 hover:bg-gray-100 hover:text-blue-700 focus:z-10 focus:ring-4 focus:ring-gray-100">
            Закрыть
          </button>
        </div>
      </div>
    </div>
  </div>
</template>
