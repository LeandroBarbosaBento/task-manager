<script setup>
import { useForm, router } from '@inertiajs/vue3';
import { ref, computed } from 'vue';
import EditTaskModal from './Task/EditTaskModal.vue';

const props = defineProps({
    title: {
        type: String,
        default: "",
    },
    tasks: {
        type: Array,
        default: () => [],
    },
    id: {
        type: [Number, String],
        default: null,
    },
});

const showCompleted = ref(false);
const dropdownOpen = ref(null);

const selectedTask = ref(null);
const isEditTaskModalOpen = ref(false);

const pendingTasks = computed(() => props.tasks.filter(t => !t.done));
const completedTasks = computed(() => props.tasks.filter(t => t.done));

const form = useForm({
    newTask: '',
});

const addTask = () => {
    if (form.newTask.trim()) {
        form
            .transform(() => ({
                title: form.newTask.trim(),
                task_list_id: props.id,
            }))
            .post(route('task.store'), {
                onSuccess: () => {
                    form.reset();
                    form.clearErrors();
                },
            });
    }
};

const onRemoveTask = (task) => {
    router.delete(route('task.destroy', task.id));
    dropdownOpen.value = null;
};

const onEditClick = (task) => {
    selectedTask.value = task;
    isEditTaskModalOpen.value = true;
    dropdownOpen.value = null;
};

const emit = defineEmits(['add', 'remove']);

const teste = () => {
    console.log('teste');
    console.log(props.tasks);
}
</script>

<template>
    <EditTaskModal
        :isOpen="isEditTaskModalOpen"
        :task="selectedTask"
        @close="isEditTaskModalOpen = false"
    />
    <div class="inline-block max-w-sm min-w-[20rem] bg-white rounded-lg p-2 px-4 m-2 border border-gray-200 max-h-[75vh]">
        <h3 class="text-md font-medium text-blue-600 my-2" @click="teste">
            {{ title }}
        </h3>

        <form
            class="flex flex-col gap-1 mb-3"
            @submit.prevent="addTask"
        >
            <div class="flex items-center gap-2">
                <input
                    v-model="form.newTask"
                    type="text"
                    placeholder="New task"
                    class="flex-1 rounded-xl border border-gray-300 px-2 py-1 text-sm focus:ring-blue-500 focus:border-blue-500"
                    @keydown.enter.prevent="addTask"
                />
                <button
                    type="submit"
                    title="Add task"
                    class="bg-blue-600 text-white px-1 py-1 rounded-2xl hover:bg-blue-700 transition flex items-center justify-center"
                    :disabled="form.processing"
                >
                    <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 13l4 4L19 7" />
                    </svg>
                </button>
            </div>
            <span
                v-if="form.newTask"
                class="text-xs text-gray-500 ml-1"
            >
                Press enter to save
            </span>
            <span v-if="form.errors.newTask" class="text-xs text-red-600 ml-1">
                {{ form.errors.newTask }}
            </span>
            <span v-if="form.errors.title" class="text-xs text-red-600">{{ form.errors.title }}</span>
        </form>

        <ul
            role="list"
            class="divide-y divide-gray-100 overflow-y-auto max-h-[60vh]"
        >
            <li
                v-for="task in pendingTasks"
                :key="task.id"
                class="flex items-center justify-between gap-x-6 py-3 px-2 rounded whitespace-normal hover:bg-gray-100 transition"
            >
                <div class="flex justify-between w-full">
                    <div class="flex items-center gap-x-3">
                        <input
                            type="checkbox"
                            :id="'task-' + task.id"
                            v-model="task.done"
                            class="h-5 w-5 rounded-xl border-gray-300 text-blue-600 focus:ring-blue-500"
                        />
                        <label
                            :for="'task-' + task.id"
                            class="text-sm font-medium text-gray-700"
                        >
                            {{ task.title }}
                        </label>
                    </div>
                    <div class="relative dropdown-menu">
                        <button
                            class="ml-2 text-gray-400 hover:text-blue-600 transition rotate-90"
                            title="Task options"
                            @click.stop="dropdownOpen = dropdownOpen === task.id ? null : task.id"
                            type="button"
                        >
                            <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                                <circle cx="12" cy="12" r="2" />
                                <circle cx="19" cy="12" r="2" />
                                <circle cx="5" cy="12" r="2" />
                            </svg>
                        </button>
                        <div
                            v-if="dropdownOpen === task.id"
                            class="absolute right-0 z-10 mt-2 w-28 rounded-md bg-white shadow-lg ring-1 ring-black/5 py-1"
                        >
                            <button
                                class="block w-full text-left px-4 py-2 text-sm text-gray-700 hover:bg-gray-100"
                                @click="onEditClick(task)"
                            >
                                Edit
                            </button>
                            <button
                                class="block w-full text-left px-4 py-2 text-sm text-red-600 hover:bg-gray-100"
                                @click="onRemoveTask(task)"
                            >
                                Remove
                            </button>
                        </div>
                    </div>
                </div>
            </li>

            <li class="py-2">
                <button
                    class="flex items-center text-sm text-gray-600 hover:text-gray-800 focus:outline-none"
                    @click="showCompleted = !showCompleted"
                >
                    <svg
                        :class="{'rotate-90': showCompleted}"
                        class="h-4 w-4 mr-2 transition-transform"
                        fill="none"
                        viewBox="0 0 24 24"
                        stroke="currentColor"
                    >
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5l7 7-7 7" />
                    </svg>
                    <span>
                        {{ showCompleted ? 'Hide' : 'Display' }} completed tasks ({{ completedTasks.length }})
                    </span>
                </button>
                <ul v-if="showCompleted" class="mt-2 space-y-1">
                    <li
                        v-for="task in completedTasks"
                        :key="task.id"
                        class="flex items-center gap-x-3 whitespace-normal"
                    >
                        <div class="flex justify-between w-full">
                            <div class="flex items-center gap-x-3">
                                <input
                                    type="checkbox"
                                    :id="'task-' + task.id"
                                    v-model="task.done"
                                    :true-value="1"
                                    :false-value="0"
                                    class="h-5 w-5 rounded-xl border-gray-300 text-blue-400 focus:ring-blue-500"
                                />
                                <label
                                    :for="'task-' + task.id"
                                    class="text-sm font-medium text-gray-400 line-through"
                                >
                                    {{ task.title }}
                                </label>
                            </div>
                            <button
                                class="ml-2 text-gray-400 hover:text-red-600 transition"
                                title="Remove task"
                                @click="$emit('remove', task)"
                                type="button"
                            >
                                <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                                        d="M6 18L18 6M6 6l12 12" />
                                </svg>
                            </button>
                        </div>
                    </li>
                </ul>
            </li>
        </ul>
    </div>
</template>
