<script setup>
  import {ref} from 'vue';
  let todos = ref([])
  let input = ref([''])
  function a() {
    todos.value.push({
      text: input.value,
      complete: false
    })
    input.value = ''
  }
  function deleted(index) {
    todos.value.splice(index, 1)
  }
</script>

<template>
  <h1>My todo</h1>
  <ol>
  <li v-for="(todos, index) in todos">
    <input type="checkbox" v-model="todos.complete">
    {{ todos.text }} 
    <button @click="deleted(index)">X</button>
  </li>
  </ol>
  <br>
  <input v-model="input" @keydown.enter="a">
  <button @click="a()">Add Todo</button>
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
</style>