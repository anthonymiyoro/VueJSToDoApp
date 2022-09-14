<script setup>
  import {ref, onMounted, computed, watch} from 'vue'

  const todos = ref([])
  const name = ref('')

  const input_content = ref('')
  const input_category = ref(null)

  // Sort the todo items
  const todos_asc = computed(() => todos.value.sort((a, b) => {
    return b.createdAt - a.createdAt
  }))

  const addTodo = () => {
    if (input_content.value.trim() === '' || input_category.value === null){
      return
    }
    console.log("addTodo");

    todos.value.push({
      content: input_content.value,
      category: input_category.value,
      done: false,
      createdAt: new Date().getTime()
    })

    input_category.value = null
    input_content.value = ''

  }

  // Delete a todo value
  const removeTodo = todo => {
    todos.value = todos.value.filter(t => t != todo)
  }

  // Save the todo Items into localstorage as a JSON object
  watch(todos, newVal => {
    localStorage.setItem('todos', JSON.stringify(newVal))
  }, {deep: true})

  // Save the name to local storage
  watch(name, (newVal)=>{
    localStorage.setItem('name', newVal)
  })

  // Collect the name from localStorage
  onMounted(() =>{
    name.value = localStorage.getItem('name') || ''
    todos.value = JSON.parse(localStorage.getItem('todos')) || []
  })
</script>

<template>
  <main class="app">
    <section class="greeting">
      <h2 class="title">
        What's up, <input type="text" placeholder="Name here" v-model="name"/>
      </h2>
    </section>

    <section class="create-todo">
      <h3>CREATE A TODO</h3>

      <form @submit.prevent="addTodo"> <!-- The submit event will no longer reload the page -->
        <h4>What's on your todo list?</h4>
        <input
          type="text"
          placeholder="e.g. make a video"
          v-model="input_content" />    <!-- The input will be attatched to input content -->
          
          {{ input_content }}


        <h4>Pick a category</h4>
        <div class="options">
          <label>
            <input
              type="radio"
              name="category"
              value="business"
              v-model="input_category"/> <!-- The input will be attatched to input content -->
              <span class="bubble business"></span>
              <div>Business</div>
          </label>

          <label>
            <input
              type="radio"
              name="category"
              value="personal"
              v-model="input_category"/> <!-- The input will be attatched to input content -->
              <span class="bubble personal"></span>
              <div>Personal</div>
          </label>

          {{ input_category }}

        </div>

        <input type="submit" value="Add todo" />
      </form>
    </section>

    <section class="todo-list">
      <h3>TODO LIST</h3>
      <div class="list">
        <div v-for="todo in todos_asc" :class="`todo-item ${todo.done && 'done'}`">

          <label>
            <input type="checkbox" v-model="todo.done" />
            <span :class="`bubble ${todo.category}`"></span>
          </label>

          <div class="todo-content">
            <input type="text" v-model="todo.content"/>
          </div>

          <div class="actions">
            <button class="delete" @click="removeTodo(todo)">Delete</button>
          </div>
        </div>

      </div>

    </section>

  </main>

</template>