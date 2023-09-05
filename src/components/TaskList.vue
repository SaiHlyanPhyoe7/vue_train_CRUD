<template>
  <div
    v-for="(item, index) in props.todoList"
    :key="index"
    class="flex justify-between px-12 bg-indigo-600 w-1/2 text-white py-2 rounded-md mb-1 mx-auto items-center"
  >
    <h2>{{ item.task }}</h2>
    <h3 v-if="item.complete === true" class="text-emerald-400">Complete</h3>
    <h3 v-else class="text-orange-400">In Progress</h3>
    <button class="bg-red-500 px-2 py-1 rounded-md" @click="handleDelete(index)">Delete</button>
    <button class="bg-blue-400 px-2 py-1 rounded-md" @click="showModal(index)">Update</button>
  </div>
  <div>
    <a-modal v-model:open="open" title="Update Task" class="flex justify-start items-center">
      <a-input v-model:value="updateTask" />
      <a-checkbox v-model:checked="updateCheck" class="">Complete</a-checkbox>

      <a-button
        class="bg-emerald-400 mt-2 border border-emerald-400 shadow-lg text-white"
        type=""
        @click="handleModelUpdate"
        >Submit</a-button
      >
    </a-modal>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'

const emit = defineEmits(['delete-todo', 'update-todo'])
const open = ref<boolean>(false)
const updateTask = ref<string>('')
const updateIndex = ref<number>()
const updateCheck = ref<boolean>()

const showModal = (index: number) => {
  open.value = true
  updateIndex.value = index
}

const handleModelUpdate = () => {
  emit('update-todo', updateTask.value, updateIndex.value, updateCheck.value)
  open.value = false
  updateTask.value = ''
  updateCheck.value = false
}

interface FormState {
  task: string
  complete: boolean
}

const props = defineProps({
  todoList: {
    type: Array as () => FormState[]
  }
})
const handleDelete = (index: number) => {
  console.log('Handle Delete with index:', index)
  emit('delete-todo', index)
}
</script>
