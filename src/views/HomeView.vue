<template>
  <div class="flex flex-col justify-start items- mt-20">
    <FormCompo @todo-list-values="handleSubmit" />
  </div>
</template>
<script lang="ts" setup>
import FormCompo from '../components/FormCompo.vue'
const handleSubmit = (values: any) => {}
</script>
