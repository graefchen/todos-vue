<script setup>
import { computed, ref } from "vue";
import TaskForm from "./components/TaskForm.vue";
import TaskList from "./components/TaskList.vue";

const message = ref("Task App");
const tasks = ref([]);

const totalDone = computed(() =>
  tasks.value.reduce((total, task) => (task.done ? +1 : total), 0)
);

function addTask(newTask) {
  tasks.value.push({
    id: crypto.randomUUID(),
    title: newTask,
    done: false,
  });
}

function toggleDone(id) {
  const task = tasks.value.find((task) => task.id === id);
  if (task) {
    task.done = !task.done;
  }
}

function removeTask(id) {
  const index = tasks.value.findIndex((task) => task.id === id);
  if (index !== -1) {
    tasks.value.splice(index, 1);
  }
}
</script>

<template>
  <main>
    <h1>{{ message }}</h1>
    <TaskForm @add-task="addTask" />
    <h3 v-if="!tasks.length">Add a task to get started.</h3>
    <h3 v-else>{{ totalDone }} / {{ tasks.length }} tasks completed</h3>
    <TaskList :tasks @toggle-done="toggleDone" @remove-task="removeTask" />
  </main>
</template>

<style scoped>
main {
  max-width: 800px;
  margin: 1rem auto;
}
</style>
