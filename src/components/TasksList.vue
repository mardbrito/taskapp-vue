<script setup lang="ts">
defineProps({
  tasks: {
    type: Object,
    required: true,
  },
});
defineEmits(["delete-task"]);
</script>

<template>
  <section class="w-[350px]">
    <article v-for="task in tasks" key="task.id">
      <div
        class="flex items-center justify-between gap-3 border-b border-base-300 py-2"
      >
        <div class="flex items-center text-left gap-3">
          <input
            type="checkbox"
            v-model="task.done"
            class="checkbox checkbox-primary"
          />
          <span :class="[task.done ? 'line-through text-primary ' : '']">
            {{ task.title }}
          </span>
        </div>
        <button
          @click="$emit('delete-task', task.id)"
          class="btn btn-circle btn-ghost"
        >
          <svg
            xmlns="http://www.w3.org/2000/svg"
            class="h-4 w-4"
            fill="none"
            viewBox="0 0 24 24"
            stroke="currentColor"
          >
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              stroke-width="2"
              d="M6 18L18 6M6 6l12 12"
            />
          </svg>
        </button>
      </div>
    </article>
  </section>
</template>
