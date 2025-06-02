<script setup>
import AuthenticatedLayout from '@/Layouts/AuthenticatedLayout.vue';
import { Head } from '@inertiajs/vue3';
import { ref, computed } from 'vue';
import TaskList from '@/Components/TaskList.vue';
import Modal from '@/Components/Modal.vue';
import CreateTaskListModal from '@/Components/Task/CreateTaskListModal.vue';
import Notification from '@/Components/Notification.vue';

defineProps({ taskLists: Array })

const isCreateTaskListModalOpen = ref(false);

const onTaskListCreated = () => {
    isCreateTaskListModalOpen.value = false;
    // Optionally, you can refresh the task lists or fetch them again

}

</script>

<template>
    <Head title="Dashboard" />

    <Notification
        :message="$page.props.flash.success"
        type="success"
    />

    <Notification
        :message="$page.props.flash.error"
        type="error"
    />

    <AuthenticatedLayout>
        <template #header>
            <div class="flex items-center justify-between">
                <h2 class="font-semibold text-xl text-gray-800 leading-tight">Dashboard</h2>
                <button
                    class="ml-4 bg-blue-600 text-white px-4 py-2 rounded hover:bg-blue-700 transition"
                    @click="isCreateTaskListModalOpen = true"
                >
                    Create task list
                </button>
            </div>

            <CreateTaskListModal
                :isOpen="isCreateTaskListModalOpen"
                @saved="isCreateTaskListModalOpen = false"
                @close="isCreateTaskListModalOpen = false"
            />

        </template>

        <div class="flex py-2 overflow-x-auto scrollbar-none whitespace-nowrap">
            <TaskList
                v-for="taskList in taskLists"
                :key="taskList.id"
                :id="taskList.id"
                :title="taskList.title"
                :tasks="taskList.tasks"
            />
        </div>
    </AuthenticatedLayout>
</template>
