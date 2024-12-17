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
  <section>
    <article v-for="task in tasks" key="task.id">
      <label>
        <input type="checkbox" v-model="task.done" />
        <span :class="{ done: task.done }">
          {{ task.title }}
        </span>
      </label>
      <button @click="$emit('delete-task', task.id)">remove</button>
    </article>
  </section>
</template>

<style scoped>
.done {
  text-decoration: line-through;
}
</style>
