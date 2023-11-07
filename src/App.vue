<script setup>
import {ref} from "vue";

// Create refs to store state
const newTodo = ref('');
const todos = ref([]);

// Add new todo
const addTodo = () => {
   // Trim input and validate
  if (newTodo.value.trim() === '')
    return;

  // Create new todo object
  todos.value.push({text: newTodo.value, completed: false, editMode: false});
  newTodo.value = ''
}

// Remove todo by index
const removeTodo = (index) => {
  todos.value.splice(index, 1)
}

// Toggle completed state
const toggleComplete = (todo) => {
  todo.completed = !todo.completed
}

// Toggle edit mode
const toggleEditMode = (todo) => {
  todo.editMode = !todo.editMode;
}

// Save updated text on enter press
const saveUpdatedTodo = (todo) => {
  toggleEditMode(todo);
}

</script>

<template>
 <div class="container">
    <h1>Todo List</h1>
    <div class="todo-container">
      <input type="text" v-model="newTodo" @keyup.enter="addTodo" placeholder="Add a new todo.">
      <ul class="todo-list">
        <li v-for="(todo, index) in todos" :key="index">
          <div class="todo">
            <div :class="{ 'is-completed': todo.completed }">
              <span v-if="!todo.editMode">{{ todo.text }}</span>
              <input
                v-else
                type="text"
                v-model="todo.text"
                @keyup.enter="saveUpdatedTodo(todo)"
                @blur="toggleEditMode(todo)"
              />
            </div>
            <div class="icons">
              <div class="icons">
                <a @click="toggleComplete(todo)"><i class=" icon fas fa-check"></i></a>
                <a @click="toggleEditMode(todo)"><i class="icon fas fa-pen"></i></a>
              </div>
              <a class="delete" @click.prevent="removeTodo(index)" href="#" type="submit">Delete</a>
            </div>
          </div>
        </li>
      </ul>
    </div>
  </div>

</template>

<style scoped>
h1 {
  text-align: center;
  margin-top: 2rem;
}

.todo-container {
  width: 60%;
  margin: 0 auto;
}

input {

}

.todo-list {
  margin: 0 auto;
}

li {
  list-style: none;
  margin-left: -1rem;
}

.todo {
  border: 1px solid #ccc;
  padding: 1rem;
  border-radius: 5px;
  display: flex;
  justify-content: space-between;
}

.icons {
  display: flex;
  align-items: center;
}

.icons a {
  text-decoration: none;
  color: #000;
  margin-left: 1rem;
}

.delete:hover {
  color: red;
}

.icon {
  cursor: pointer;
  margin-left: 1rem;
}

.is-completed {
  color: lightgreen;
  font-weight: bold;
}

.todo input {
  border: none;
  background-color: transparent;
  font-weight: bold;
  color: inherit;
}
</style>
