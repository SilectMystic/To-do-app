<script setup>
  import {ref, watch} from 'vue';
  let todos = ref(JSON.parse(window.localStorage.getItem('todos'))?? [])
  let input = ref([''])
  let filter = ref([])
  watch(todos, function(value) {
      window.localStorage.setItem('todos', JSON.stringify(value))
    }, {deep: true})
  function a() {
    todos.value.push({
      text: input.value,
      complete: false
    })
    input.value = ''
    window.localStorage.setItem('todos', todos.value)
  }
  function deleted(index) {
    todos.value.splice(index, 1)
  }
  function todosFilter(todos) {
    if(filter.value == "active" ){
      return todos.complete == false
     } else if (filter.value == 'inactive'){
      return todos.complete == true
     }
      else{
        return true
      }
     }
    
</script>

<template>
  <h1>My todo</h1>
  <br>
  <input type="radio" name="filter" value="all" v-model="filter">
  <label>All</label>
  &nbsp;
  <input type="radio" name="filter" value="active" v-model="filter">
  <label>Active</label>
  &nbsp;
  <input type="radio" name="filter" value="inactive" v-model="filter">
  <label>Complete</label>
  <br>
  <ol>
  <li v-for="(todos, index) in todos.filter(todosFilter)" :class="{complete: todos.complete}">
    <input type="checkbox" v-model="todos.complete">
    {{ todos.text }} 
    <button class="deleted" @click="deleted(index)">X</button>
  </li>
  </ol>
  <br>
  <input v-model="input" @keydown.enter="a">
  <button @click="a()" id="submit">Add Todo</button>
</template>

<style>
  @import url('https://fonts.googleapis.com/css2?family=Nunito&display=swap');

  body {
    background-color: #0f0e17;
    color: white;
    font-family: 'Nunito', sans-serif;
    text-align: center;
  }
  ol {
    text-align: center;
    display: inline-block;
  }
  li {
    text-align: center;
  }
  .complete {
    text-decoration: line-through;
    color: orange;
  }
  .deleted {
    display: none;
  }
  li:hover > .deleted {
    display: inline-block;
  }
  input[type="checkbox"] {
    color: orange;
    background-color: red;
    height: 30px;
    width: 30px;
    cursor: pointer;
    -webkit-appearance: none;
  }
  input[type="checkbox"]:after {
    background-color: aliceblue;
    padding: 100px;
    -webkit-appearance: button;

  }
  #submit {
    cursor: pointer;
  }
</style>