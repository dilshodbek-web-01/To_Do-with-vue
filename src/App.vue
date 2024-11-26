<template>
  <header class="bg-emerald-800 p-2">
    <nav class="flex items-center justify-center">
      <h4 class="text-white">To Do App</h4>
    </nav>
  </header>
  <main>
    <section>
      <div class="container">
        <form
          @submit.prevent="addTask"
          action="#"
          class="p-6 w-1/3 mx-auto mt-10 rounded-md bg-slate-200"
        >
          <label for="#" class="w-full">
            <p>Enter task title</p>
            <input
              type="text"
              class="w-full mb-5 py-1 px-3 border rounded"
              placeholder="Enter title ..."
              v-model="taskTitle"
            />
          </label>
          <label for="#">
            <p>Enter task date</p>
            <input
              type="date"
              class="w-full py-1 px-3 border rounded"
              v-model="taskDate"
            />
          </label>
          <button
            type="submit"
            class="text-white focus:bg-green-500 bg-emerald-800 w-full py-1 mt-5 rounded"
          >
            ADD TASK
          </button>
        </form>
        <div
          v-if="!tasks.length"
          class="p-3 w-1/3 mx-auto my-7 rounded-md bg-slate-200 text-center"
        >
          <p class="text-yellow-500 text-xl">Not Found Tasks!</p>
        </div>
        <ul
          v-else
          v-for="el in tasks"
          class="p-2 w-1/3 mx-auto my-4 rounded-md bg-slate-200"
        >
          <li class="bg-white rounded p-2 relative">
            <strong class="absolute top-0">{{ 1 }}</strong>
            <span class="flex items-center justify-between mt-4 mb-2">
              <p v-if="!el.completed">{{ el.title }}</p>
              <p v-else class="line-through">{{ el.title }}</p>
              <p class="bg-slate-200 px-1 rounded text-sm">{{ el.date }}</p></span
            >
            <button
              @click="() => deleteTask(el.id)"
              class="bg-red-800 text-white rounded px-2"
            >
              Delete
            </button>
            <button
              @click="() => taskComplate(el.id)"
              :disabled="el.completed"
              class="bg-green-800 disabled:bg-green-400 text-white rounded px-2 ms-5"
            >
              Done
            </button>
          </li>
        </ul>
      </div>
    </section>
  </main>
  <footer class="bg-emerald-800 text-white text-center p-2 mt-[200px]">
    <p>{{ new Date().getFullYear() }}</p>
  </footer>
</template>

<script setup>
import { ref } from "vue";
import { v4 as uuidv4 } from "uuid";
import { toast } from 'vue3-toastify';
import 'vue3-toastify/dist/index.css';

const taskTitle = ref("");
const taskDate = ref("");
const tasks = ref([]);

const addTask = () => {
  if (taskTitle.value.trim().length && taskDate.value.length) {
    const newTask = {
      id: uuidv4(),
      created_at: Date.now(),
      title: taskTitle.value,
      date: taskDate.value,
      completed: false,
    };
    tasks.value.push(newTask);
    taskTitle.value = "";
    taskDate.value = "";
    toast("Task successfully added", {
        autoClose: 2500,
      });
  } else {
    toast.warning("Task title and date are required", {
        autoClose: 2500,
      });
  }
};

const deleteTask = (id) => {
  tasks.value = tasks.value.filter((el) => el.id != id);
  toast("Task successfully deleted", {
      autoClose: 2500,
    });
};

const taskComplate = (id) => {
  tasks.value.forEach((el) => {
    if (el.id === id) {
      el.completed = !el.completed;
    }
  });
  toast("Task completed", {
      autoClose: 2500,
    });
};
</script>
