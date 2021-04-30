<template>
  <h1>Vue3 Todo List - シン レヴァン</h1>
  <form @submit.prevent="addNewTodo">
    <label>新し ToDo</label>
    <br>
    <input v-model='newTodo' name="newTodo">
    <br>
    <button>追加</button>
  </form>
  <button @click='markAllDone'>すべて完了になる</button>
  <br>
  <button @click='removeAll'>すべて削除</button>
  <ul class="todoList">
    <li v-for="(todo, index) in todos" :key='todo.id' class="todo">
      <h3 :class='{done : todo.isDone}'  @click="toggleDone(todo)">{{todo.content}}</h3>
      <button @click='removeTodo(index)'>削除</button>
    </li>
  </ul>
  
</template>

<script>
import {ref} from 'vue';

export default {
  setup() {
    const newTodo = ref('');
    const todos = ref([]); // the array in which we will append the to do's

    function addNewTodo() {
      todos.value.push(
        {
          id: Date.now(), //Only made to give to the key in line 11 so that it can have a unique value
          isDone: false, // the state of the task, will change to true when the task is completed
          content : newTodo.value
        }
      );
      newTodo.value = '';
    }

    function toggleDone(todo) {
      todo.isDone = !todo.isDone;
    }

    function removeTodo(index) {
      todos.value.splice(index , 1);
    }

    function removeAll() {
      todos.value = [];
    }

    function markAllDone() {
      todos.value.forEach((todo) => todo.isDone = true);
    }
    
    // to use these in the template we have to return them here 
    return {
      addNewTodo,
      newTodo,
      todos,
      toggleDone,
      removeTodo, 
      removeAll,
      markAllDone,  
    };
  }

}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  margin-left: 1%;
  color: #2c3e50;
  margin-top: 60px;

}

button {
  margin-top: 20px;
}

input {
  margin-top: 20px;
  width: 30%;
  height: 20px;
}
/* the following class is specified so that we can get the line through style if the task is donee */
.done {
  text-decoration: line-through;
}

.todo {
  cursor: pointer;
}

.todoList {
  text-align: center;
}

</style>
