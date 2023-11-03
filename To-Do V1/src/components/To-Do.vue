<script setup>
import { ref, reactive } from 'vue'

const newTask = ref('')
const todos = reactive([
  { task: "Do Programming", completed: true },
  { task: "Go to Outing", completed: false },
  { task: "Pay Bill", completed: true },
  { task: "Do Exercise", completed: true }
])

const deletedTasks = reactive([])

const addTask = () => {
  if (newTask.value.trim() !== '') {
    todos.unshift({ task: newTask.value, completed: false })
    newTask.value = ''
  }
}

const editTask = (index) => {
  if (index >= 0 && index < todos.length) {
    todos[index].editing = true
    todos[index].originalText = todos[index].task
  }
}

const saveTask = (index) => {
  if (index >= 0 && index < todos.length) {
    todos[index].editing = false
    localStorage.setItem('todos', JSON.stringify(todos))
  }
}

const cancelEdit = (index) => {
  if (index >= 0 && index < todos.length) {
    todos[index].editing = false
    todos[index].task = todos[index].originalText
  }
}

const doCheck = (index) => {
  todos[index].completed = true
}

const undoCheck = (index) => {
  todos[index].completed = false
}

const deleteTask = (index) => {
  const deletedTask = todos.splice(index, 1)[0]
  deletedTasks.push(deletedTask)
}
</script>


<template>
  <div class="min-h-screen flex flex-col justify-center items-center">
    <div class="w-full max-w-screen-lg bg-white overflow-hidden">
      <div class="px-4">
        <div class="w-full max-w-2xl mx-auto">
          <div class="pb-3 text-center">
            <h1 class="text-4xl font-extrabold text-gray-900">Simple To-Do App</h1>
          </div>

          <div class="flex space-x-2">
            <input v-model="newTask" @keyup.enter="addTask" placeholder="Add a new task" class="flex-grow rounded-md p-2 border border-gray-300 focus:outline-none focus:ring focus:border-blue-300" />

            <button @click="addTask" class="px-4 py-2 bg-blue-500 text-white rounded-md hover:bg-blue-600">Add</button>
          </div>

          <table class="mt-4 w-full min-w-full border border-collapse">
            <thead>
              <tr>
                <th class="w-2.5/4 py-2 px-4 border">Serial</th>
                <th class="w-1.5/4 py-2 px-4 border">Text</th>
                <th class="w-1.5/4 py-2 px-4 border">Actions</th>
              </tr>
            </thead>

            <tbody>
              <tr v-for="(todo, index) in todos" :key="index">
                <td class="py-2 px-4 border">{{ index + 1 }}.</td>
                
                <td :class="{'line-through': todo.completed}" v-if="!todo.editing" class="py-2 px-4 border">{{ todo.task }}</td>

                <td v-else class="py-2 px-4 border">
                  <input v-model="todo.task" @keyup.enter="saveTask(index)" @keyup.esc="cancelEdit(index)" class="flex-grow rounded-md p-2 border border-gray-300 focus:outline-none focus:ring focus:border-blue-300" />
                </td>

                <td class="flex justify-center items-center">
    
                    <td class="py-2 px-4 border">
                      <button @click="saveTask(index)" v-if="todo.editing" class="px-2 py-1 w-20 bg-blue-500 text-white rounded-md hover:bg-blue-600">Save</button>
                      <button @click="editTask(index)" v-if="!todo.editing" class="px-2 py-1 w-20 bg-blue-500 text-white rounded-md hover:bg-blue-600">Edit</button>
                    </td>

                    <td class="py-2 px-4 border">
                      <button @click="doCheck(index)" v-if="!todo.completed" class="px-2 py-1 w-20 bg-green-500 text-white rounded-md hover:bg-green-600">Check</button>
                      <button @click="undoCheck(index)" v-if="todo.completed" class="px-2 py-1 w-20 bg-red-500 text-white rounded-md hover:bg-red-600">Undo</button>
                    </td>


                    <td class="py-2 px-4 border">
                      <button @click="deleteTask(index)" v-if="!todo.completed" class="px-2 py-1 w-20 bg-yellow-500 text-white rounded-md hover:bg-yellow-600">Delete</button>
                      <button @click="deleteTask(index)" v-if="todo.completed" class="px-2 py-1 w-20 bg-yellow-500 text-white rounded-md hover:bg-yellow-600">Delete</button>
                    </td>
                </td>
              </tr>
            </tbody>
          </table>

          <div class="container mx-auto p-4 bg-yellow-300 text-center">
              <h2 class="text-3xl font-semibold mb-4">
                  You can find me on 
                  <a href="https://github.com/ashekgo" class="text-blue-500 hover:underline" target="_blank" rel="noopener noreferrer">GitHub</a>
                  and
                  <a href="https://www.linkedin.com/in/ashekgo" class="text-blue-500 hover:underline" target="_blank" rel="noopener noreferrer">LinkedIn</a>
              </h2>
          </div>

        </div>
      </div>
    </div>
  </div>
</template>