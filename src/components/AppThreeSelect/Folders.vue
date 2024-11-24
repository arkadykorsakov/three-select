<script setup lang="ts">
import {IFolder} from "../../types";
import RadioButton from "./RadioButton.vue";
import {ref, watch} from "vue";

defineProps<{ folders: IFolder[], name: string }>()
const activeFolder = ref<null | number>(null)

function toggleFolder(folder: number): void {
  activeFolder.value = activeFolder.value === folder ? null : folder
}

const value = defineModel()

watch(value, () => {
  activeFolder.value = null
})
</script>

<template>
  <ul class="text-sm text-gray-700">
    <template v-for="(folder, idx) in folders" :key="folder.id">
      <li v-if="!folder.children?.length" class="first:rounded-t-lg last:rounded-b-lg overflow-hidden">
        <RadioButton :name="name" :value="folder.id.toString()" :label="folder.name" v-model="value"
                     :checked="value === folder.id.toString()"/>
      </li>
      <li v-else :class="{t:idx === 0, b:idx === folders.length - 1}">
        <div class="relative">
          <div
              class="radio flex justify-between w-full overflow-hidden">
            <RadioButton :name="name" :value="folder.id.toString()" :label="folder.name" v-model="value"
                         :checked="value === folder.id.toString()"/>
            <button class="flex items-center rtl:rotate-180 hover:bg-gray-200 p-2 flex-auto" type="button"
                    @click="toggleFolder(folder.id)">
              <svg class="w-2.5 h-2.5 " aria-hidden="true" xmlns="http://www.w3.org/2000/svg"
                   fill="none" viewBox="0 0 6 10">
                <path stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                      d="m1 9 4-4-4-4"/>
              </svg>
            </button>
          </div>
          <div
              v-if="activeFolder === folder.id"
              class="absolute top-0 left-[calc(100%+10px)] z-10 bg-white divide-y divide-gray-100 rounded-lg shadow w-44">
            <ul class="text-sm text-gray-700 dark:text-gray-200" aria-labelledby="doubleDropdownButton">
              <li>
                <Folders v-if="folder.children?.length" :folders="folder.children" v-model="value" :name="name"/>
              </li>
            </ul>
          </div>
        </div>
      </li>
    </template>
  </ul>
</template>

<style scoped>
.t > div > .radio {
  @apply rounded-t-lg;
}

.b > div > .radio {
  @apply rounded-b-lg;
}
</style>
