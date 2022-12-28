<script setup>
import { ref, computed, watch } from "vue"

const todosData = ref([
  {
    text: 'LAVAR ROPA',
    priority: 1,
    done: false,
    id: 'z'
  },
  {
    text: 'COMPRAR COMIDA AL GATO',
    priority: 2,
    done: false,
    id: 'x'
  },
  {
    text: 'IR AL SUPER',
    priority: 1,
    done: true,
    id: 'c'
  }
])

const sortingKey = ref('done')

const sortedTodos = ref(todosData.value)
const sortedTodosLength = computed(() => sortedTodos.value.length)
// computed(() => {
//   if (sortingKey.value === 'priority') {
//     console.log("todos 1", todosData.value);
//     const todos = sortByPriority(todosData.value)
//     console.log("todos 2", todosData.value);
//     return todos;
//   }
//   return todosData.value
// });

const sortByStatus = () => {
  return todosData.value.sort((a) => {
    if (a.done) {
      return 1
    } else {
      return -1
    }
  })
}

const sortByPriority = () => {
  return todosData.value.sort((a, b) => {
    if (a['priority'] < b['priority']) {
      return 1
    } else {
      return -1
    }
  })
}

const sortTodos = (sortingKey) => {
  if (sortingKey === 'priority') sortedTodos.value = sortByPriority();
  if (sortingKey === 'done') sortedTodos.value = sortByStatus()
}

watch(sortingKey, (newVal, oldVal) => {
  if (newVal === oldVal) return
  sortTodos(newVal);
  return
}, { immediate: true })


watch(sortedTodosLength, (newVal, oldVal) => {
  if (newVal > oldVal) sortTodos(sortingKey.value);
});

const todoInputValue = ref('');
const newTodoPriority = ref(1);

const addTodo = () => {
  const newTodo = {
    priority: newTodoPriority.value,
    text: todoInputValue.value,
    done: false,
    id: `${new Date().getTime()}`
  }
  console.log("todoInputValue", todoInputValue.value)
  todosData.value.push(newTodo);
  todoInputValue.value = ''
}
</script>

<template>
  <div class="TodoView">
    <h1>To-do list:</h1>

    <div @click="todo.done = !todo.done" v-for="todo in sortedTodos" class="to-do" :class="{ done: todo.done }"
      :key="todo.id">
      {{ todo.text }}, prioridad {{ todo.priority }}
    </div>

    <div>
      <h2>Add a Todo to the list!</h2>
      <div>
        <label for="todoInput">To-do:</label>
        <input name="todoInput" v-model="todoInputValue" type="text" @keypress.enter="addTodo">
      </div>
      <div>
        <label for="todoPriorityInput">Priority:</label>
        <input name="todoPriorityInput" v-model="newTodoPriority" type="number" @keypress.enter="addTodo">
      </div>

      <div>
        <label for="">Sort by:</label>
        <div>
          <input v-model="sortingKey" type="radio" id="priority" name="priority" value="priority">
          <label for="priority">Priority</label>
        </div>

        <div>
          <input v-model="sortingKey" type="radio" id="done" name="done" value="done">
          <label for="done">Status</label>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.to-do {
  color: black;
  background: white;
  padding: 1rem;
  border: transparent 5px solid;
}

.to-do:hover {
  cursor: pointer;
  background: hsla(160, 100%, 37%, 1);
  color: white;
}

.done {
  text-decoration: line-through;
}

@media (min-width: 1024px) {
  .TodoView {
    min-height: 100vh;
    display: flex;
    flex-direction: column;
    justify-content: start;
    padding: 30% 0;
  }
}
</style>
