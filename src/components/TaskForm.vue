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
  <form @submit.prevent="formSubmit">
    <label>
      Task
      <input type="text" v-model="task" />
    </label>
    <button type="submit">Add</button>
  </form>
</template>
