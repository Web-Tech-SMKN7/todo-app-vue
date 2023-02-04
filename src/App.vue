<script setup>
import { computed, onBeforeMount, ref, watch } from 'vue';

const newTask = ref('')
const tasks = ref([
  {
    title: "Ngoding sambil kayang",
    done: false,
  },
  {
    title: "Ngoding sambil kayang tapi ngodingnya pake bahasa Pascal",
    done: false,
  },
])
const totalTaskDone = computed(() => tasks.value.filter(task => task.done).length)
const totalTaskOngoing = computed(() => tasks.value.filter(task => !task.done).length)

function newTaskAction() {
  tasks.value.push({
    title: newTask.value,
    done: false
  })

  newTask.value = ''
}

function deleteTask(index) {
  tasks.value.splice(index, 1)
}

onBeforeMount(() => {
  const json = localStorage.getItem('tasks')
  
  if (!json) return

  tasks.value = JSON.parse(json)
})

watch(tasks, () => {
  localStorage.setItem('tasks', JSON.stringify(tasks.value))
},
{ deep: true })
</script>

<template>
  <div class="wrapper">
    <div id="todo-app">
      <header>
        <h1>To-do List</h1>
      </header>
      <main>
        <form id="add-group" @submit.prevent="newTaskAction">
          <input v-model="newTask"
            type="text" name="input" id="add-input">
          <button id="add">Add</button>
        </form>
        <div class="aggregate-group">
          <div>
            <b>Ongoing</b>
            <span class="aggregate" id="task-ongoing">{{ totalTaskOngoing }}</span>
          </div>
          <div>
            <b>Done</b>
            <span class="aggregate" id="task-done">{{ totalTaskDone }}</span>
          </div>
          <div>
            <b>Total</b>
            <span class="aggregate" id="task-total">{{ tasks.length }}</span>
          </div>
        </div>

        <div id="task-list">
          <div class="task" v-for="(task, index) in tasks">
            <input v-model="task.done" 
              :id="`task-${index}__check`"
              class="task__check" 
              type="checkbox" 
              >
            <label 
              class="task__title" 
              :for="`task-${index}__check`"
            >
              {{ task.title }}
            </label>
            <div class="task__delete" @click="() => deleteTask(index)">Delete</div>
          </div>
        </div>
      </main>
    </div>
  </div>
</template>

<style>
body {
  margin: 0;
  padding: 0;
}
</style>
