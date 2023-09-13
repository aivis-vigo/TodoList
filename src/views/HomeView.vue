<script setup lang="ts">
import {ref} from "vue";

const inputValue = ref('');

interface Task {
    id: number;
    title: string;
    isFinished: boolean;
}

const tasks = ref(<Task[]>[]);
const finishedTasks = ref(<Task[]>[]);

const handleSubmit = () => {
    if (inputValue.value.trim() !== "") {
        tasks.value.push(
            {
                id: tasks.value.length + 1,
                title: inputValue.value,
                isFinished: false
            }
        )
        inputValue.value = '';
    }
}

const toggleCheckbox = (id: number) => {
    const result = tasks.value.filter(task => task.id === id);

    result[0].isFinished = !result[0].isFinished;

    finishedTasks.value.push(result[0]);
    deleteTask(result[0]);
}

const deleteTask = (task: Task) => {
    for (let i = 0; i < tasks.value.length; i++) {
        if (tasks.value[i] === task) {
            tasks.value.splice(i, 1);
        }
    }
}
</script>

<template>
    <main class="h-screen bg-gradient-to-r from-blue-950 to-gray-800 text-white">
        <div class="mx-auto w-1/2">
            <h2 class="flex justify-center text-4xl font-bold mb-4">Todo App</h2>
            <form @submit.prevent="handleSubmit">
                <label for="createTask" class="mb-2 text-sm font-medium text-gray-900 sr-only dark:text-white">
                    Take a Walk
                </label>
                <div class="relative">
                    <div class="absolute inset-y-0 left-0 flex items-center pl-3 pointer-events-none">
                        <svg class="h-6 w-6 text-gray-500"
                             fill="none"
                             viewBox="0 0 24 24"
                             stroke="currentColor">
                            <path stroke-linecap="round"
                                  stroke-linejoin="round"
                                  stroke-width="2"
                                  d="M12 4v16m8-8H4"/>
                        </svg>
                    </div>
                    <input v-model="inputValue"
                           type="text"
                           id="createTask"
                           class="block w-full p-4 pl-10 text-sm text-gray-900 border border-gray-300 rounded-lg bg-gray-50 focus:ring-blue-500 focus:border-blue-500 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
                           placeholder="Take a Walk">
                    <button type="submit"
                            class="text-white absolute right-2.5 bottom-2.5 bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:outline-none focus:ring-blue-300 font-medium rounded-lg text-sm px-4 py-2 dark:bg-blue-600 dark:hover:bg-blue-700 dark:focus:ring-blue-800">
                        Create
                    </button>
                </div>
            </form>


            <div class="flex justify-center mt-4">
                <ul class="text-xl w-full divide-y divide-gray-200">
                    <li v-for="task in tasks" :key="task.id" class="pb-3 sm:pb-4">
                        <div @click="toggleCheckbox(task.id)" class="flex items-center space-x-4">
                            <div class="flex-shrink-0">
                                <input type="checkbox" :checked="task.isFinished">
                            </div>
                            <div class="flex-1 min-w-0">
                                <label class="font-medium text-gray-900 truncate dark:text-white">
                                    {{ task.title }}
                                </label>
                            </div>
                            <form @submit.prevent="deleteTask(task)"
                                  class="inline-flex items-center text-base font-semibold text-red-500">
                                <button type="submit">
                                    Delete
                                </button>
                            </form>
                        </div>
                    </li>
                </ul>
            </div>

            <div class="text-white">
                <h2 class="text-2xl font-bold">Finished Tasks</h2>
                <ul class="text-xl w-full divide-y divide-gray-200">
                    <li v-for="task in finishedTasks" :key="task.id" class="pb-3 sm:pb-4">
                        <div @click="toggleCheckbox(task.id)" class="flex items-center space-x-4">
                            <div class="flex-shrink-0">
                                <input type="checkbox" :checked="task.isFinished">
                            </div>
                            <div class="flex-1 min-w-0">
                                <label class="font-medium text-gray-900 truncate dark:text-white">
                                    {{ task.title }}
                                </label>
                            </div>
                            <form @submit.prevent="deleteTask(task)"
                                  class="inline-flex items-center text-base font-semibold text-red-500">
                                <button type="submit">
                                    Delete
                                </button>
                            </form>
                        </div>
                    </li>
                </ul>
            </div>
        </div>
    </main>
</template>
