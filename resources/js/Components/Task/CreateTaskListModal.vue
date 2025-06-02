<script setup>
import Modal from '@/Components/Modal.vue';
import { ref } from 'vue';
import { useForm } from '@inertiajs/vue3'
import Notification from '@/Components/Notification.vue';

defineProps({
    isOpen: {
        type: Boolean,
        default: false,
    },
});

const emit = defineEmits(['close', 'saved']);

const form = useForm({
    title: '',
})

const notification = ref({
    message: '',
    type: 'success',
    isDisplaying: true,
});

const onClose = () => {
    emit('close');
    form.reset();
    form.clearErrors()
}

const handleSubmit = () => {

    if (form.title.length < 3) {
        form.errors.title = 'Tasklist title must be at least 3 characters long';
        return;
    }

    form.post(
        route("tasklist.store"),
        {
            onSuccess: () => {
                form.reset('title');

                notification.value.type = 'success';
                notification.value.message = 'Tasklist created successfully!';

                emit('saved');
            },
            onError: (errors) => {
                notification.value.type = 'error';
                notification.value.message = 'Failed to create tasklist.';

                form.errors = errors;
            },
        }
    );
}

</script>

<template>

    <Modal
        :show="isOpen"
        max-width="sm"
        @close="onClose"
    >
        <form @submit.prevent="handleSubmit" class="p-4 flex flex-col gap-3">
            <label class="text-sm font-medium text-gray-700">
                Tasklist title
                <input
                    v-model="form.title"
                    type="text"
                    required
                    class="mt-1 block w-full rounded border border-gray-300 px-2 py-1 text-sm focus:ring-blue-500 focus:border-blue-500"
                />
            </label>
            <span v-if="form.errors.title" class="text-xs text-red-600">{{ form.errors.title }}</span>
            <div class="flex justify-end gap-2">
                <button
                    type="button"
                    class="px-3 py-1 rounded bg-gray-200 text-gray-700 hover:bg-gray-300"
                    @click="onClose"
                >
                    Cancel
                </button>
                <button
                    type="submit"
                    class="px-3 py-1 rounded bg-blue-600 text-white hover:bg-blue-700"
                >
                    Save
                </button>
            </div>
        </form>
    </Modal>
</template>

