<script setup>
  import {ref, onMounted, computed, watch} from 'vue'
  const  todo = ref([])
  const name = ref('')

  const input_content = ref('')
  const input_category = ref(null)

  const todos_asc = computed(()=>todo.value.sort((a,b) => {
    return b.createdA-a.createdA
  }))

  const addTodo = ()=> {
    if (input_content.value.trim() === '' || input_category.value.trim() === null) {
      return
    }

   todo.value.push({
    content: input_content.value,
    category: input_category.value, 
    done:false,
    createdA: new Date().getTime()
   })
  }
watch (todo, newVal => {
  localStorage.setItem(todo,JSON.stringify(newVal))
})
  watch(name, (newVal) => {
    localStorage.setItem('name', newVal)
  }, {deep:true})

  onMounted(()=>{
    name.value = localStorage.getItem("name")
    todo.value = JSON.parse(localStorage.getItem('todo')) || []
  })
</script>


<template>
 <main class="app">
  <div class="section">
    <h2 class="title">
      Quoi de new <input type="text" v-model="name">
      
    </h2>
  </div>
  <div class="create-todo">
    <h3>CREATED A TODO</h3>
    <form @submit.prevent="addTodo">
      <h2>What on you Todo lis?</h2>
      <input type="text" 
      placeholder="e.g. make a video" 
      v-model="input_content"/>
      {{ input_content }}
      <h4>Pick a categorie</h4>
      <div class="option">
        <label for="">
          <input type="radio" 
          name="category" 
          id="category1" 
          value="business" 
          v-model="input_category">
          <span class="buble">
            <div>Busness</div>
          </span>
        </label>

        <label for="">
          <input type="radio" 
          name="category" 
          id="personal" 
          value="Personl" 
          v-model="input_category">
          <span class="buble">
            <div>Personal</div>
          </span>
        </label>

        {{ input_category }}
      </div>
      <input type="submit" value="Add todo">
    </form>
  </div>


  <section class="to-list">
    <h3>TODO LIST</h3>
    <div v-for="tod in todos_asc" :class="`todo-item  ${tod.done && 'done'}`">
    <label for=""></label>
  </div>
  </section>
 </main>
</template>

