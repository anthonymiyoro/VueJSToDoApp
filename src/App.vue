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

  // Save the name to local storage
  watch(name, (newVal)=>{
    localStorage.setItem('name', newVal)
  })

  // Collect the name from localStorage
  onMounted(() =>{
    name.value = localStorage.getItem('name') || ''
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

  </main>

</template>
