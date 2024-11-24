<script setup lang="ts">
import {useId} from "vue";
interface IProps {
  name: string;
  value: string;
  label: string;
  checked?: boolean;
}
defineProps<IProps>()
const modelValue = defineModel()
const id = useId()
</script>

<template>
  <label :for="id" class="relative w-full h-full p-2 block hover:bg-gray-100" :class="{'bg-blue-300 hover:bg-blue-400 text-white':checked}">
    <input type="radio" class="absolute inset-0 opacity-0 cursor-pointer"
           :value="value" v-model="modelValue"
           :name="name" :id="id"/>
    {{ label }}
  </label>
</template>
