<script setup>
  import {ref, watch} from 'vue';
  let todos = ref(JSON.parse(window.localStorage.getItem('todos'))?? [])
  let input = ref([''])
  let filter = ref([])
  watch(todos, function(value) {
      window.localStorage.setItem('todos', JSON.stringify(value))
    }, {deep: true})
  function a() {
    if (input.value == ""){
      console.log("nothing")
    }
    else{
      todos.value.push({
      text: input.value,
      complete: false
    })
    input.value = ''
    window.localStorage.setItem('todos', todos.value)
    }
  }
  function deleted(index) {
    todos.value.splice(index, 1)
  }
  function clearDel(todos) {
    if (todos.complete != 'inactive'){
      todos.complete=todos.complete.filter(todos.complete=true)
    }
    else{
      console.log('no we')
    }
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
  <div id="box">
    <div id="title">
      <h1>To-Do App</h1>
    </div>
    <hr>
    <div v-if="todos.length > 0" id="filters">
      <label class="radioCon">All
      <input type="radio" checked="checked" name="filter" v-model="filter" value="all">
      <span class="radioCheck"></span>
      </label>
      &nbsp;
      <label class="radioCon">Active
      <input type="radio" name="filter" v-model="filter" value="active">
      <span class="radioCheck"></span>
      </label>
      &nbsp;
      <label class="radioCon">complete
      <input type="radio" name="filter" v-model="filter" value="inactive">
      <span class="radioCheck"></span>
      </label>
    </div>
    <div v-if="todos.length == 0" id="addTodo">
      <p>
        Start by adding a To-Do!
      </p>
    </div>
    <div id="list" v-if="todos.length > 0">
      <hr>
      <ol>
      <li v-for="(todos, index) in todos.filter(todosFilter)" :class="{complete: todos.complete}">
        <label class="container">
        <input type="checkbox" checked="checked" v-model="todos.complete">
        <span class="checkmark"></span>
        </label>
        &nbsp;
        &nbsp;
        &nbsp;
        &nbsp;
        {{ todos.text }}
        &nbsp; 
        <button class="deleted" @click="deleted(index)">X</button>
      </li>
      </ol>
    </div>
    <hr>
    <div id="input">
      <p v-if="todos.length == 1">
        {{ todos.filter(activeFilter).length}} Item Left
      </p>
      <p v-if="todos.length >= 2">
        {{ todos.filter(activeFilter).length}} Items Left
      </p>
      <p v-if="todos.length == 0">
        <br>
      </p>
      <input v-model="input" @keydown.enter="a" placeholder="Add todo!">
      <br> <br>
      <button @click="a()" id="submit">Add Todo</button>
      <br><br>
    </div>
  </div>
</template>

<style>
  @import url('https://fonts.googleapis.com/css2?family=Nunito&display=swap');

  body {
    background-color: #0f0e17;
    color: white;
    font-family: 'Nunito', sans-serif;
    text-align: center;
    margin-left: auto;
    margin-right: auto;
    max-width: 1080px;
    margin-top: 8%;
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
  background-color: #232946;
  border-radius: 8px;
}

/* On mouse-over, add a grey background color */
.container:hover input ~ .checkmark {
  background-color: rgb(34, 32, 32);
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
    background-color: #232946;
    padding: 5px;
    border-radius: 8px;
    text-decoration: none;
    color: white;
}

button:hover {
    background-color: rgb(34, 32, 32);
    color: #6246ea;
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

/* The container */
.radioCon {
  display: inline-block;
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
.radioCon input {
  position: absolute;
  opacity: 0;
  cursor: pointer;
}

/* Create a custom radio button */
.radioCheck {
  position: absolute;
  top: 0;
  left: 0;
  height: 25px;
  width: 25px;
  background-color: #232946;
  border-radius: 50%;
}

/* On mouse-over, add a grey background color */
.radioCon:hover input ~ .radioCheck {
  background-color: rgb(34, 32, 32);
}

/* When the radio button is checked, add a blue background */
.radioCon input:checked ~ .radioCheck {
  background-color: #2196F3;
}

/* Create the indicator (the dot/circle - hidden when not checked) */
.radioCon:after {
  content: "";
  position: absolute;
  display: none;
}

/* Show the indicator (dot/circle) when checked */
.radioCon input:checked ~ .radioCheck:after {
  display: inline-block;
}

/* Style the indicator (dot/circle) */
.radioCon .radioCheck:after {
 	top: 9px;
	left: 9px;
	width: 8px;
	height: 8px;
	border-radius: 50%;
	background: white;
}

#addTodo {
  padding: 15px;
  background-color: #151232;
  width: 200px;
  margin: auto;
  border-radius: 8px;
  margin-top: 35px;
  margin-bottom: 34px;
}
#box {
  background-color: #1b2139;
  padding: 10px;
  border-radius: 8px;
}
#title {
  background-color: #3f4d85;
  padding: 10px;
  border-radius: 8px;
}
#filters {
  background-color: #262f52;
  padding: 15px;
  border-radius: 8px;
  padding-bottom: 5px;
  padding-top: 18px;
}
hr {
  padding: 0px;
  margin: 2px;
  border: #7f5af0 1px solid;
  border-radius: 8;
}
#input {
  background-color: #121629;
  border-radius: 8px;
  padding: 5px;
  font-size: 18px;
}
#list {
  background-color: #151232;
  border-radius: 8px;
}
.deleted {
  cursor: pointer;
}
</style>