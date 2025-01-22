<script setup lang="ts">
import { useModal } from './hooks/useModal.ts'
import AppModal from './components/AppModal.vue'
import { IFolder } from './types'
import { ref } from 'vue'

const { isOpen, closeModal, openModal } = useModal()

const mockFolders: IFolder[] = [
  {
    id: 1,
    name: 'Папка 1',
    children: [
      { id: 2, name: 'Папка 1.1', children: [] },
      {
        id: 3,
        name: 'Папка 1.2',
        children: [{ id: 4, name: 'Папка 1.2.1', children: [] }]
      },
      {
        id: 6,
        name: 'Папка 1.3',
        children: [{ id: 7, name: 'Папка 1.3.1', children: [] }]
      }
    ]
  },
  { id: 5, name: 'Папка 2', children: [] }
]

const folder = ref<null | string>(null)

function setFolder(folderId: string | null) {
  folder.value = folderId
  closeModal()
}
</script>

<template>
  <main class="w-full min-h-dvh flex items-center justify-center">
    <div class="flex flex-col justify-center">
      <div v-if="folder" class="w-full p-4 bg-red-500 text-white mb-5">
        ID FOLDER: {{ folder }}
      </div>
      <button
        class="block text-white bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:outline-none focus:ring-blue-300 font-medium rounded-lg text-sm px-5 py-2.5 text-center dark:bg-blue-600 dark:hover:bg-blue-700 dark:focus:ring-blue-800"
        type="button"
        @click="openModal"
      >
        Открыть
      </button>
    </div>
    <Teleport to="body" v-if="isOpen">
      <AppModal
        name="folders"
        title="Заголовок"
        :folders="mockFolders"
        @close="closeModal"
        @select="setFolder"
        :active-folder="folder"
      />
    </Teleport>
  </main>
</template>
