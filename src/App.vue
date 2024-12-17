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
  <div class="bg-base-100 flex justify-center">
    <div class="text-center max-h-screen mt-10">
      <div class="max-w-lg">
        <h1 class="text-5xl font-bold mb-6">{{ title }}</h1>

        <TaskForm @add-task="(title) => addTask(title)" />

        <div v-if="tasks.length" class="my-5 space-y-2">
          <p class="badge badge-primary">
            {{ tasksDone }} of {{ tasks.length }}
            {{ tasks.length > 1 ? "tasks" : "task" }} completed
          </p>
          <div class="flex gap-3 items-center justify-center">
            <button
              @click="() => filterTask('all')"
              class="badge badge-outline"
            >
              all
            </button>
            <button
              @click="() => filterTask('done')"
              class="badge badge-outline"
            >
              done
            </button>
            <button
              @click="() => filterTask('todo')"
              class="badge badge-outline"
            >
              todo
            </button>
          </div>
        </div>
        <div v-else role="alert" class="alert mt-5">
          <svg
            xmlns="http://www.w3.org/2000/svg"
            fill="none"
            viewBox="0 0 24 24"
            class="stroke-info h-6 w-6 shrink-0"
          >
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              stroke-width="2"
              d="M13 16h-1v-4h-1m1-4h.01M21 12a9 9 0 11-18 0 9 9 0 0118 0z"
            ></path>
          </svg>
          <p>Add a task to get started</p>
        </div>
        <TasksList
          :tasks="filteredTasks"
          @delete-task="(id) => deleteTask(id)"
        />
      </div>
    </div>
  </div>
</template>
