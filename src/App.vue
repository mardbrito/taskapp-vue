<script setup lang="ts">
import { computed, ref } from "vue";
import { Filter, Task } from "./types/types";

import TaskForm from "./components/TaskForm.vue";
import TasksList from "./components/TasksList.vue";

const title = "Task App";

const tasks = ref<Task[]>([]);
const currentFilter = ref<Filter>("all");

const tasksDone = computed(() => {
  return tasks.value.reduce((total, task) => total + Number(task.done), 0);
});

const filteredTasks = computed(() => {
  const items =
    currentFilter.value === "all"
      ? (item: any) => item
      : currentFilter.value === "done"
      ? (item: any) => item.done
      : (item: any) => !item.done;

  return tasks.value.filter(items);
});

function addTask(newTask: string) {
  tasks.value.push({
    id: crypto.randomUUID(),
    title: newTask,
    done: false,
  });
}

function deleteTask(id: string) {
  tasks.value = tasks.value.filter((task) => task.id !== id);
}

function filterTask(type: Filter) {
  return (currentFilter.value = type);
}
</script>

<template>
  <h1>{{ title }}</h1>

  <div v-if="tasks.length">
    <p>
      {{ tasksDone }} of {{ tasks.length }}
      {{ tasks.length > 1 ? "tasks" : "task" }} completed
    </p>
    <div>
      <button @click="() => filterTask('all')">all</button>
      <button @click="() => filterTask('done')">done</button>
      <button @click="() => filterTask('todo')">todo</button>
    </div>
  </div>
  <div v-else>
    <p>Add a task to get started</p>
  </div>
  <TaskForm @add-task="(title) => addTask(title)" />
  <TasksList :tasks="filteredTasks" @delete-task="(id) => deleteTask(id)" />
</template>

<style scoped></style>
