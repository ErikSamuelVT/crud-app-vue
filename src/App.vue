<script setup>
import { ref } from 'vue'

let id = 1
let isEditing = false
let idEditTask = null
const tasks = ref([])
const task = ref('')

function createTask() {
  tasks.value.push({ "id": id++, "text": task.value })
  task.value = ''
}

function deleteTask(id) {
  tasks.value = tasks.value.filter(tsk => tsk.id !== id)
}

function editTask(id) {
  isEditing = true
  idEditTask = id
  const currentTask = tasks.value.filter(tsk => tsk.id === id)
  task.value = currentTask[0].text

}

function updateTask() {
  isEditing = false
  const editedTask = tasks.value.map(
    tsk => tsk.id === idEditTask ?
      { "id": idEditTask, "text": task.value } :
      tsk
  )
  tasks.value = editedTask
  task.value = ''

}

</script>

<template>
  <main class="flex justify-center align-middle" v-if="isEditing === false">
    <div class="sm:w-2/5 md:w-3/5 lg:w-2/5 bg-slate-200 my-3 drop-shadow-md rounded-md">
      <div class="p-2">
        <input v-model="task" placeholder="Write some text"
          class="sm:w-3/5 md:w-4/5 bg-zinc-100 py-2 px-2 rounded outline-1 outline outline-zinc-300 focus:outline-none focus:drop-shadow-md">
        <button v-if="isEditing === false"
          class="sm:w-20 md:w-20 bg-slate-400 rounded-md m-1 py-2 px-1 text-white hover:bg-zinc-500 hover:drop-shadow-md"
          @click="createTask">Add</button>
        <button v-else
          class="sm:w-20 md:w-20 bg-slate-400 rounded-md m-1 py-2 px-1 text-white hover:bg-zinc-500 hover:drop-shadow-md"
          @click="updateTask">Update</button>
      </div>
      <div class="p-2">
        <ul>
          <li v-for="ts in tasks" :key="ts.id"
            class="bg-slate-300 py-2 px-3 my-1 rounded-md flex justify-between align-middle">
            <div class="flex items-center">
              <h3 class="font-bold mr-3">{{ ts.id }}</h3>
              <p>{{ ts.text }}</p>
            </div>
            <div>
              <button class="w-28 bg-blue-500 text-white rounded-md py-2 px-1" @click="editTask(ts.id)">Edit</button>
              <button class="w-28 bg-red-500 text-white rounded-md py-2 px-1 ml-1"
                @click="deleteTask(ts.id)">Delete</button>
            </div>
          </li>
        </ul>
      </div>
    </div>
  </main>
  <main class="flex justify-center align-middle" v-else>
    <div class="w-3/5 bg-slate-200 my-3 drop-shadow-md rounded-md">
      <div class="p-2">
        <input v-model="task" placeholder="Write some text"
          class="w-4/5 bg-zinc-100 py-2 px-2 rounded outline-1 outline outline-zinc-300 focus:outline-none focus:drop-shadow-md">
        <button v-if="isEditing === false"
          class="w-36 bg-slate-400 rounded-md m-1 py-2 px-1 text-white hover:bg-zinc-500 hover:drop-shadow-md"
          @click="createTask">Add</button>
        <button v-else
          class="w-36 bg-slate-400 rounded-md m-1 py-2 px-1 text-white hover:bg-zinc-500 hover:drop-shadow-md"
          @click="updateTask">Update</button>
      </div>
    </div>
  </main>
</template>