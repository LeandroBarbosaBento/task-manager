<script setup>
import AuthenticatedLayout from '@/Layouts/AuthenticatedLayout.vue';
import { Head } from '@inertiajs/vue3';
import { ref, computed } from 'vue';
import TaskList from '@/Components/TaskList.vue';

defineProps({ taskLists: Array })

</script>

<template>
    <Head title="Dashboard" />

    <AuthenticatedLayout>
        <template #header>
            <h2 class="font-semibold text-xl text-gray-800 leading-tight">Dashboard</h2>
        </template>

        <div class="flex py-2 overflow-x-auto scrollbar-none whitespace-nowrap">
            <TaskList
                v-for="taskList in taskLists"
                :key="taskList.id"
                :title="taskList.title"
                :tasks="taskList.tasks"
            />
        </div>
    </AuthenticatedLayout>
</template>
