<script setup>
  import {ref} from 'vue';
  let todos = ref([])
  let input = ref([''])
  function a() {
    todos.value.push(input.value)
    input.value = ''
  }
  function deleted(index) {
    todos.value.splice(index, 1)
  }
</script>

<template>
  <h1>My todo</h1>
  <ul>
  <li v-for="(todos, index) in todos"> 
    {{ index }}
    {{ todos }} 
    <button @click="deleted(index)">X</button>
  </li>
  </ul>
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
  ul {
    text-align: center;
    display: inline-block;
  }
  li {
    text-align: center;
  }
</style>