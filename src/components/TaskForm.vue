<script setup lang="ts">
import { ref } from "vue";

const emit = defineEmits(["add-task"]);
const task = ref("");

function formSubmit() {
  if (!task.value.trim()) return;

  emit("add-task", task.value);
  task.value = "";
}
</script>

<template>
  <form @submit.prevent="formSubmit" class="max-w-lg">
    <input
      type="text"
      v-model="task"
      placeholder="Type your new task here"
      class="input input-bordered w-full placeholder-gray-700"
    />
  </form>
</template>
