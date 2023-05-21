<script setup>
  import {ref, watch} from 'vue';
  let todos = ref(JSON.parse(window.localStorage.getItem('todos'))?? [])
  let input = ref([''])
  let filter = ref([])
  let filCom = ref([filter.value == 'inactive'])
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
  function clearDel() {
    filCom.delete()
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
  function activeFilter(todos) {
    return todos.complete == false
  }    
</script>

<template>
  <head>
    
  </head>
  <h1>My todo</h1>
  <br>
  <div v-if="todos.length > 0">
    <input type="radio" name="filter" value="all" v-model="filter">
    <label>All</label>
    &nbsp;
    <input type="radio" name="filter" value="active" v-model="filter">
    <label>Active</label>
    &nbsp;
    <input type="radio" name="filter" value="inactive" v-model="filter">
    <label>Complete</label>
  </div>
  <br>
  <ol>
  <li v-for="(todos, index) in todos.filter(todosFilter)" :class="{complete: todos.complete}">
    <label class="container">
    <input type="checkbox" checked="checked" v-model="todos.complete">
    <span class="checkmark"></span>
    </label>
    &nbsp;
    &nbsp;
    &nbsp;
    {{ todos.text }}
    &nbsp; 
    <button class="deleted" @click="deleted(index)">X</button>
  </li>
  </ol>
  <br>
  <p>
    {{ todos.filter(activeFilter).length}} Items Left
  </p>
  <input v-model="input" @keydown.enter="a" placeholder="Add todo!">
  <br> <br>
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
    color: #2e375e;
  }
  /* .deleted {
    display: none;
  }
  li:hover > .deleted {
    display: inline-block;
  } */
  /* input[type="checkbox"] {
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

  } */
  #submit {
    cursor: pointer;
  }
  .container {
  display: block;
  position: relative;
  padding-left: 35px;
  margin-bottom: 12px;
  cursor: pointer;
  font-size: 22px;
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
}

/* Hide the browser's default checkbox */
.container input {
  position: absolute;
  opacity: 0;
  cursor: pointer;
  height: 0;
  width: 0;
}

/* Create a custom checkbox */
.checkmark {
  position: absolute;
  top: 0;
  left: 0;
  height: 25px;
  width: 25px;
  background-color: #eee;
}

/* On mouse-over, add a grey background color */
.container:hover input ~ .checkmark {
  background-color: #ccc;
}

/* When the checkbox is checked, add a blue background */
.container input:checked ~ .checkmark {
  background-color: #2196F3;
}

/* Create the checkmark/indicator (hidden when not checked) */
.checkmark:after {
  content: "";
  position: absolute;
  display: none;
}

/* Show the checkmark when checked */
.container input:checked ~ .checkmark:after {
  display: block;
}

/* Style the checkmark/indicator */
.container .checkmark:after {
  left: 9px;
  top: 5px;
  width: 5px;
  height: 10px;
  border: solid white;
  border-width: 0 3px 3px 0;
  -webkit-transform: rotate(45deg);
  -ms-transform: rotate(45deg);
  transform: rotate(45deg);
}
button {
    background-color: #b8c1ec;
    padding: 5px;
    border-radius: 8px;
    text-decoration: none;
}

button:hover {
    background-color: rgb(34, 32, 32);
    color: #124aff;
}
input {
    background-color: #040318;
    color: #fffffe;
    font-family: 'Nunito', sans-serif;
    border: 3px;
    border-color: #2e375e;
    border-style: solid;
    border-radius: 9px;
    padding: 8px;

}

input:focus {
    border: rgb(34, 32, 32) 5px solid;
}
.radio {
  display: block;
  position: relative;
  padding-left: 35px;
  margin-bottom: 12px;
  cursor: pointer;
  font-size: 22px;
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
}

/* Hide the browser's default radio button */
.radio input {
  position: absolute;
  opacity: 0;
  cursor: pointer;
  height: 0;
  width: 0;
}

/* Create a custom radio button */
.radiocheck {
  position: absolute;
  top: 0;
  left: 0;
  height: 25px;
  width: 25px;
  background-color: #eee;
  border-radius: 50%;
}

/* On mouse-over, add a grey background color */
.radio:hover input ~ .radiocheck {
  background-color: #ccc;
}

/* When the radio button is checked, add a blue background */
.radio input:checked ~ .radiocheck {
  background-color: #2196F3;
}

/* Create the indicator (the dot/circle - hidden when not checked) */
.radiocheck:after {
  content: "";
  position: absolute;
  display: none;
}

/* Show the indicator (dot/circle) when checked */
.radio input:checked ~ .radiocheck:after {
  display: block;
}

/* Style the indicator (dot/circle) */
.radio .radiocheck:after {
  top: 9px;
  left: 9px;
  width: 8px;
  height: 8px;
  border-radius: 50%;
  background: white;
}
</style>