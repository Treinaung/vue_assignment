<script setup>
import HelloWorld from './components/HelloWorld.vue'
import TheWelcome from './components/TheWelcome.vue'

import { ref, onMounted, computed, watch } from 'vue'

const todos = ref([])
const name = ref('')

const input_content = ref('')


const todos_asc = computed(() => todos.value.sort((a, b) => {
  return a.createdAt - b.createdAt
}))


watch(name, (newVal) => {
  localStorage.setItem('name', newVal)
})


watch(todos, newVal => {
  localStorage.setItem('todos', JSON.stringify(newVal))
}, { deep: true })

const addList = () => {
  if (input_content.value.trim() === '' ) {
    return
  }

  todos.value.push({
    content: input_content.value,   
    done: false,
    editable: false,
    createdAt: new Date().getTime()
  })
}

const removeTodo = (todo) => {
	todos.value = todos.value.filter((t) => t !== todo)
}


onMounted(() => {
  name.value = localStorage.getItem('name') || ''
  todos.value = JSON.parse(localStorage.getItem('todos')) || []
})

</script>

<template>
  <header>
    <img alt="Vue logo" class="logo" src="./assets/logo.svg" width="125" height="125" />

    <div class="wrapper">
      <HelloWorld msg="Vue Assignment" />
    </div>
  </header>

  <main class ="app">

  <h1 v-if="showWelcome">Hello</h1>
    <h1 v-else>Hi</h1>
    <button @click="celebrateWelcome" style="color: white; background-color: green;">Click</button>
    <hr>

    <section class="Listing">
        <h3>Listing</h3>

        <form id="new-todo-form" @submit.prevent="addList">
          <h4>Enter List</h4>
          <input type="text" name="content" id="content" placeholder="list" v-model="input_content" />
          
          

          <input type="submit" value="Add Name" style="color: white; background-color: blue;" />
          
        </form>
    </section><br><hr><br>

    <section class="the lists">
      <h3>The List</h3>
      <div class="list">
        <div v-for="todo in todos_asc" :class="`todo-item ${todo.done && 'done'}`">

        <br>

        <div class="todo-content">
          <input type="text" v-model="todo.content" />
        </div>

        <br>

        <div class="actions">
						<button class="delete" @click="removeTodo(todo)" style="color: white; background-color: red;">Delete</button>
				</div>

        </div>
      </div>
    </section>

    
    
    
  </main>
  
</template>

<script >


export default {
  data() {
    return {
      message: "We Love Web Sys Class.",
      count: 0,
      showWelcome: true,
      
    }
  },
  created() {
    console.log('Created');
  },
  methods: {
    increateNumber(){
      console.log("Call increate Method");
      this.count += 1;
    },
    celebrateWelcome() {
      this.showWelcome = false;
    }
  }
}

</script>

<style scoped>
header {
  line-height: 1.5;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }
}
</style>
