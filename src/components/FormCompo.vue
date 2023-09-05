<template>
  <div class="flex justify-center items-center">
    <a-form
      class="bg-gray-200 shadow-md p-12 mb-2 rounded-md flex justify-start gap-x-4 items-center"
      :model="formState"
      name="basic"
      :label-col="{ span: 8 }"
      :wrapper-col="{ span: 16 }"
      autocomplete="off"
      @finish="onFinish"
      @finishFailed="onFinishFailed"
    >
      <a-form-item
        label="Task Name :"
        name="task"
        :rules="[{ required: true, message: 'Please input your task!' }]"
        class=""
      >
        <a-input v-model:value="formState.task" />
      </a-form-item>

      <a-form-item name="complete" :wrapper-col="{ span: 16 }" class="">
        <a-checkbox v-model:checked="formState.complete" class="">Complete</a-checkbox>
      </a-form-item>

      <a-form-item :wrapper-col="{ offset: 8, span: 16 }" class="">
        <a-button
          class="bg-emerald-400 border border-emerald-400 shadow-lg text-white"
          type=""
          html-type="submit"
          >Submit</a-button
        >
      </a-form-item>
    </a-form>
  </div>
  <TaskList
    @update-todo="handleUpdateFromTodo"
    @delete-todo="handleDeleteFromTodo"
    :todoList="todoList"
  />
</template>

<script lang="ts" setup>
import { reactive, ref } from 'vue'
import TaskList from './TaskList.vue'
const emit = defineEmits(['todo-list-values'])

interface FormState {
  task: string
  complete: boolean
}

const formState = reactive<FormState>({
  task: '',
  complete: false
})

const todoList = ref<FormState[]>([])

const handleDeleteFromTodo = (index: number) => {
  todoList.value.splice(index, 1)
}

const handleUpdateFromTodo = (updateTask: string, updateIndex: number, updateCheck: boolean) => {
  console.log({ updateIndex, updateTask })
  todoList.value[updateIndex].task = updateTask
  todoList.value[updateIndex].complete = updateCheck
}

const onFinish = (values: any) => {
  // Emit a custom event with the updated todoList

  emit('todo-list-values', values)
  todoList.value.push(values)
  formState.complete = false
  formState.task = ''
}

const onFinishFailed = (errorInfo: any) => {
  console.log('Failed:', errorInfo)
}
</script>
