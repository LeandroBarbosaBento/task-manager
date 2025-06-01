<script setup>
import { ref, computed } from 'vue';

const props = defineProps({
    title: {
        type: String,
        default: "",
    },
    tasks: {
        type: Array,
        default: () => [],
    },
});

const showCompleted = ref(false);

const pendingTasks = computed(() => props.tasks.filter(t => !t.done));
const completedTasks = computed(() => props.tasks.filter(t => t.done));
</script>

<template>
    <div class="inline-block max-w-sm min-w-[20rem] bg-white rounded-lg p-2 px-4 m-2 border border-gray-200 max-h-[75vh]">
        <h2 class="text-md font-medium text-blue-600 my-2">
            My list
        </h2>

        <ul
            role="list"
            class="px-2 divide-y divide-gray-100 overflow-y-auto max-h-[68vh]"
        >
            <li
                v-for="task in pendingTasks"
                :key="task.id"
                class="flex items-center justify-between gap-x-6 py-3 whitespace-normal"
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
                            {{ task.description }}
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
                                    class="h-5 w-5 rounded-xl border-gray-300 text-blue-400 focus:ring-blue-500"
                                />
                                <label
                                    :for="'task-' + task.id"
                                    class="text-sm font-medium text-gray-400 line-through"
                                >
                                    {{ task.description }}
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
