<script setup>
  import {ref, watch} from 'vue';
  let todos = ref(JSON.parse(window.localStorage.getItem('todos'))?? [])
  let input = ref([''])
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
</script>

<template>
  <h1>My todo</h1>
  <ol>
  <li v-for="(todos, index) in todos" :class="{complete: todos.complete}">
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
  .deleted:hover {
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
  input[type="checkbox"]::after {
    background-color: white;
    color: black;
    font-family: 'Nunito', sans-serif;
    -webkit-appearance: none;
    height: 100px;
    width: 50px;
  }
  #submit {
    cursor: pointer;
  }
</style>