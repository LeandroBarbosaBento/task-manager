<script setup>
import AuthenticatedLayout from '@/Layouts/AuthenticatedLayout.vue';
import { Head } from '@inertiajs/vue3';
import { ref, computed } from 'vue';
import TaskList from '@/Components/TaskList.vue';

const tasks = ref([
  { id: 1, description: 'Estudar Vue.js', done: false },
  { id: 2, description: 'Revisar código do projeto', done: false },
  { id: 3, description: 'Enviar relatório diário', done: false },
  { id: 4, description: 'Task com nome grande e descrição, há quebra de linha, teste de task', done: false },
  { id: 1, description: 'Estudar Vue.js', done: false },
  { id: 2, description: 'Revisar código do projeto', done: false },
  { id: 3, description: 'Enviar relatório diário', done: false },
  { id: 4, description: 'Task com nome grande e descrição, há quebra de linha, teste de task', done: false },
  { id: 1, description: 'Estudar Vue.js', done: false },
  { id: 1, description: 'Estudar Vue.js', done: false },
  { id: 2, description: 'Revisar código do projeto', done: false },
  { id: 3, description: 'Enviar relatório diário', done: false },
  { id: 4, description: 'Task com nome grande e descrição, há quebra de linha, teste de task', done: false },
  { id: 1, description: 'Estudar Vue.js', done: false },
  { id: 2, description: 'Revisar código do projeto', done: false },
  { id: 3, description: 'Enviar relatório diário', done: false },
  { id: 4, description: 'Task com nome grande e descrição, há quebra de linha, teste de task', done: false },
  { id: 1, description: 'Estudar Vue.js', done: false },
  { id: 1, description: 'Estudar Vue.js', done: false },
  { id: 2, description: 'Revisar código do projeto', done: false },
  { id: 3, description: 'Enviar relatório diário', done: false },
  { id: 4, description: 'Task com nome grande e descrição, há quebra de linha, teste de task', done: false },
  { id: 1, description: 'Estudar Vue.js', done: false },
  { id: 2, description: 'Revisar código do projeto', done: false },
  { id: 3, description: 'Enviar relatório diário', done: false },
  { id: 4, description: 'Task com nome grande e descrição, há quebra de linha, teste de task', done: false },
  { id: 1, description: 'Estudar Vue.js', done: false },

]);

</script>

<template>
    <Head title="Dashboard" />

    <AuthenticatedLayout>
        <template #header>
            <h2 class="font-semibold text-xl text-gray-800 leading-tight">Dashboard</h2>
        </template>

        <div class="py-2 overflow-x-auto scrollbar-none whitespace-nowrap">

            <TaskList
                title="My list"
                :tasks="tasks"
            />

            <TaskList
                title="My list"
                :tasks="tasks"
            />

            <TaskList
                title="My list"
                :tasks="tasks"
            />

            <TaskList
                title="My list"
                :tasks="tasks"
            />

        </div>
    </AuthenticatedLayout>
</template>
